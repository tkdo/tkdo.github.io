
```bash
#!/bin/bash
function exec_sql(){
   refresh_sql="$1"
   debug="$2"
   debug="${debug:=0}"
   echo "############ 打印SQL #################"
   echo    "${refresh_sql}"
   echo "############ 执行SQL ################"
   if [  ${debug} -eq "1" ];then
      hive -e "${refresh_sql}"
   fi

   if [ $? -ne 0 ]; then
      echo "############ 执行失败 ###############"
      exit 1
   else
      echo "############ 执行成功 ###############"
   fi
   echo ""
   echo ""
   echo ""
   echo ""
}

param="
    set hive.mapred.mode=nostrict;
    set hive.execution.engine=mr;
    set mapred.job.queue.name=root.ai.feednews;
    set hive.exec.script.allow.partial.consumption=true;
    set hive.exec.dynamici.partition=true;
    set hive.exec.dynamic.partition.mode=nonstrict;
    set mapred.task.timeout=18000000;
    set hive.groupby.skewindata=true;
    set hive.optimize.skewjoin=true;
    set hive.exec.parallel=true;
    set hive.ignore.mapjoin.hint=true;"

function exec_step1(){
    step1_sql="
    ${param}
    use ${database};
    --fisrt/second/分组衰减和
    drop   table tmp_tfidf_${train}_first_second_dying_sum_step1;
    create table tmp_tfidf_${train}_first_second_dying_sum_step1 as 
    select ${first},
           flag,
           ${second}, 
           sum(dying_cnt) second_dying_cnt
    from (select ${first}
                ,flag
                ,${second}
                ,act_day
                ,exp(datediff('${dt_beg}',act_day)*(${lamda}))*sum(cnt) as dying_cnt
            from tmp_tfidf_${train}_action
            where ${second} is not null and ${second} <> ''
            group by ${first}
                    ,flag
                    ,${second}
                    ,act_day
        ) t 
    group by ${first}
            ,flag
            ,${second};"       
    exec_sql "${step1_sql}" "${debug}"
} 

function exec_step2(){
    step2_sql="
    ${param}
    use ${database};
    drop   table tmp_tfidf_${train}_tf_step2;
    create table tmp_tfidf_${train}_tf_step2 as 
    --first分组之后衰减和
    with first_dying_sum as (
    select ${first}
        ,flag
        ,sum(second_dying_cnt) first_dying_cnt
    from tmp_tfidf_${train}_first_second_dying_sum_step1
    group by ${first}
        ,flag
    )
    select t1.${first}
        ,t1.flag
        ,t2.${second}
        ,t1.first_dying_cnt
        ,t2.second_dying_cnt
    from first_dying_sum as t1
    join tmp_tfidf_${train}_first_second_dying_sum_step1 as t2
    on t1.${first}=t2.${first} and t1.flag=t2.flag;"
    exec_sql "${step2_sql}" "${debug}"
}

function exec_step3(){
    step3_sql="
    ${param}
    use ${database};
    drop   table tmp_tfidf_${train}_idf_step3;
    create table tmp_tfidf_${train}_idf_step3 as 
    --second分组之后的总个数
    with second_total as (
    select ${second}
        ,flag
        ,count(${second}) second_cnt
    from tmp_tfidf_${train}_first_second_dying_sum_step1
    group by ${second}
        ,flag
    ),
    --flag分组之后的总个数
    flag_total as (
    select flag,
        count(*) as flag_cnt 
    from tmp_tfidf_${train}_first_second_dying_sum_step1
    group by flag
    )
    select /*+MAPJOIN(a) */
         b.${second}
         ,second_cnt
         ,a.flag
         ,a.flag_cnt
    from flag_total as a 
    inner join second_total as b 
    on a.flag=b.flag;"
    exec_sql "${step3_sql}" "${debug}"
}




function exec_step4(){
    step4_sql="
    ${param}
    use ${database};
    drop   table tmp_tfidf_${train};
    create table tmp_tfidf_${train} as 
    select /*+MAPJOIN(t4) */
    t3.${first}
    ,t3.${second}
    ,t3.second_dying_cnt/t3.first_dying_cnt as tf 
    ,log(t4.flag_cnt/t4.second_cnt+1) as idf 
    ,(t3.second_dying_cnt/t3.first_dying_cnt) * (log(t4.flag_cnt/t4.second_cnt+1)) as tfidf
    from tmp_tfidf_${train}_idf_step3 as t4
    join tmp_tfidf_${train}_tf_step2  as t3
    on t3.${second}=t4.${second}
    and t3.flag=t4.flag;"
    exec_sql "${step4_sql}" "${debug}"
}


function clear_steps(){
    refresh_sql="use ${database}; 
    drop table tmp_tfidf_${train}_first_second_dying_sum_step1;
    drop table tmp_tfidf_${train}_tf_step2;
    drop table tmp_tfidf_${train}_idf_step3;"
    exec_sql "${refresh_sql}" "${debug}"
}

train="$1"
first="$2"
second="$3"
debug="$4"
debug=${debug:="1"}
dt_beg="$5"
dt_beg="$(date '+%Y-%m-%d')"
lamda="$6"
lamda=${lamda:="-0.15"}
database="$7"
database=${database:="feednews"}

exec_step1
exec_step2
exec_step3
exec_step4
clear_steps
```
