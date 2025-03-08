
```python

from fastapi import FastAPI, Request
from fastapi import BackgroundTasks
import uvicorn

async def parse_param(request:Request):
    param = {}
    if request.method == "GET":
        param = request.query_params
    if request.method == "POST":
        param = await request.json()
    return dict(param)

async def home(background_tasks: BackgroundTasks, request:Request):
    print(request)
    return "hello"


app = FastAPI()
app.add_api_route(path="/", endpoint=home, methods=["POST", "GET"])



if __name__ == "__main__":
    uvicorn.run(app)
```
