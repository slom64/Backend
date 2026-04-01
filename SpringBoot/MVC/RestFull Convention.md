| Operation | API Endpoint                      | HTTP Method              | Response Status  |
| --------- | --------------------------------- | ------------------------ | ---------------- |
| Create    | ```json<br>/cashcards<br>```      | ```json<br>POST<br>```   | 201 (CREATED)    |
| Read      | ```json<br>/cashcards/{id}<br>``` | ```json<br>GET<br>```    | 200 (OK)         |
| Update    | ```json<br>/cashcards/{id}<br>``` | ```json<br>PUT<br>```    | 204 (NO CONTENT) |
| Delete    | ```json<br>/cashcards/{id}<br>``` | ```json<br>DELETE<br>``` | 204 (NO CONTENT) |
- If resource doesn't exists then the status code should be 404. Example if you don't have user 5, then `api/users/5` should return 404 not found. 
