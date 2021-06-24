# Note

## Oh my. It's isane! 😈

이렇게 간단히 세팅할 수 있다니 놀랄 수 밖에..

## Usage

Initialize environment

```shell
pipenv shell
pipenv install
# located /src
# launch server using uvicorn
uvicorn main:app --reload

INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [11582] using statreload
INFO:     Started server process [11584]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     127.0.0.1:58476 - "GET /redoc HTTP/1.1" 200 OK
INFO:     127.0.0.1:58476 - "GET /openapi.json HTTP/1.1" 200 OK
```

Now we can access api to `http://127.0.0.1:8000`.  
or request query like `http://127.0.0.1:8000/items/5?q=somequery`.  

Also fastapi automated generate document using `swagger`. let's access to `http://127.0.0.1:8000/redoc`.

![redoc screenshot from fastapi doc](https://fastapi.tiangolo.com/img/index/index-02-redoc-simple.png)

using `Pydantic` update codes. as python standard type.  

```python
from typing import Optional

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    price: float
    is_offer: Optional[bool] = None


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Optional[str] = None):
    return {"item_id": item_id, "q": q}


@app.put("/items/{item_id}")
def update_item(item_id: int, item: Item):
    return {"item_name": item.name, "item_id": item_id}
```

Then access `http://127.0.0.1:8000/docs`. you can check `swagger UI`. also test in web dashboard without using `postman`.
