# Book Swap API

## API documentation

### Return all books - optionally filtered

* **URL**

  /api/books

* **Method:**

  `GET`

* **URL Params**

  **Required:**

  There are no required URL params

  **Optional:**

  `claimed=0|1` - Either 1 or 0 to get claimed/unclaimed books
  `search=string` - A search term
  `genre=int` - Filter results by genre id

  **Example:**

  `/api/books?search=Peach&claimed=1&genre=1

* **Success Response:**

    * **Code:** 200 <br />
      **Content:** <br />

```json
{
    "data": [
        {
            "id": 2,
            "title": "foo",
            "author": "Test",
            "genre": {
                "id": 1,
                "name": "Action"
            }
        }
    ],
    "message": "Books successfully retrieved"
}
```

* **Error Response:**

    * **Code:** 422 UNPROCESSABLE CONTENT <br />
      **Content:**
        ```json
        {
            "message": "The claimed field must be a number. (and 2 more errors)",
            "errors": {
                "claimed": [
                    "The claimed field must be a number."
                ],
                "genre": [
                    "The selected genre is invalid."
                ],
                "search": [
                    "The search field must be a string."
                ]
            }
        }
        ```

### Return a specific book

* **URL**

  /api/books/{id}

* **Method:**

  `GET`

* **URL Params**

  **Required:**

  There are no required URL params

  **Optional:**

  There are no optional URL params

  **Example:**

  `/api/books/1`

* **Success Response:**

    * **Code:** 200 <br />
      **Content:** <br />

```json
{
    "data": {
        "id": 1,
        "title": "Test",
        "author": "Test person",
        "blurb": "blurb",
        "claimed_by_name": "Ash",
        "image": "https://example.com/image.jpg",
        "page_count": 1000,
        "genre": {
            "id": 1,
            "name": "Action"
        },
        "review": [
            {
                "id": 3,
                "name": "Ash",
                "rating": 1,
                "review": "bad"
            }
        ]
    },
    "message": "Book successfully found"
}
```

* **Error Response:**

    * **Code:** 404 NOT FOUND <br />
      **Content:** `{"message": "Book with id 999 not found"}`

### Claim/return a book

* **URL**

  /books/{id}

* **Method:**

  `PUT`

* **URL Params**

  **Required:**

  There are no required URL params

  **Optional:**

  There are no optional URL params

* **Body Data**

  Must be sent as JSON with the correct headers

  **Required:**

    ```json
    {
      "email": "String",
      "name": "string"
    }
    ```

  **Optional:**

    Note that the name field only needs to be sent when claiming a book. When returning a book only the email field to be sent

  **Example:**

  `/api/books`

* **Success Response:**

    * **Code:** 200 OK <br />
      **Content:** <br />

  ```json
  {"message": "Book 1 was claimed"}
  ```

    * **Code:** 200 OK <br />
      **Content:** <br />

  ```json
  {"message": "Book 1 was returned"}
  ```

* **Error Response:**

    * **Code:** 404 NOT FOUND <br />
      **Content:** `{"message": "Book 10 was not found"}`

    * **Code:** 422 UNPROCESSABLE CONTENT <br />
      **Content:** 
    ```json
    {
        "message": "The email field is required.",
        "errors": {
            "email": [
                "The email field is required."
            ]
        }
    }
    ```


### Add a new book

* **URL**

  /api/books

* **Method:**

  `POST`

* **URL Params**

  **Required:**

  There are no required URL params

  **Optional:**

  There are no optional URL params

* **Body Data**

  Must be sent as JSON with the correct headers

  **Required:**

    ```json
    {
      "title": "String",
      "author": "string",
      "genre_id": integer
    }
    ```

  **Optional:**

    ```json
    {
      "blurb": "string"
    }
    ```

  **Example:**

  `/api/books`

* **Success Response:**

    * **Code:** 201 CREATED <br />
      **Content:** <br />

  ```json
  {"message": "Book created"}
  ```

  * **Error Response:**

      * **Code:** 500 INTERNAL SERVER ERROR <br />
        **Content:** `{"message": "Unexpected error occurred"}`

      * **Code:** 422 UNPROCESSABLE CONTENT <br />
        **Content:**
      ```json
      {
          "message": "The title field is required. (and 2 more errors)",
          "errors": {
              "title": [
                "The title field is required."
              ],
              "author": [
                "The author field is required."
              ],
              "genre_id": [
                "The genre id field is required."
              ]
          }
      }
      ```

### Delete a book

* **URL**

  /api/books/{id}

* **Method:**

  `DELETE`

* **URL Params**

  **Required:**

  There are no required URL params

  **Optional:**

  There are no optional URL params

* **Body Data**

  Must be sent as JSON with the correct headers

  **Required:**

    There are no required body parameters

  **Optional:**

  There are no required body parameters

  **Example:**

  `/api/books/1`

* **Success Response:**

    * **Code:** 200 OK <br />
      **Content:** <br />

  ```json
  {"message": "Book 1 was deleted"}
  ```

    * **Error Response:**

        * **Code:** 404 NOT FOUND <br />
          **Content:** `{"message": "The title field is required. (and 2 more errors)"}`


### Add a new book review

* **URL**

  /api/reviews

* **Method:**

  `POST`

* **URL Params**

  **Required:**

  There are no required URL params

  **Optional:**

  There are no optional URL params

* **Body Data**

  Must be sent as JSON with the correct headers

  **Required:**

    ```json
    {
      "name": "String",
      "rating": integer,
      "review": "string",
      "book_id": "int"
    }
    ```
    Note: rating must be an integer between 0 and 5
* 
  **Optional:**

    There are no optional body parameters

  **Example:**

  `/api/reviews`

* **Success Response:**

    * **Code:** 201 CREATED <br />
      **Content:** <br />

  ```json
  {"message": "Review created"}
  ```

* **Error Response:**

    * **Code:** 500 INTERNAL SERVER ERROR <br />
      **Content:** `{"message": "Unexpected error occurred"}`

    * **Code:** 422 UNPROCESSABLE CONTENT <br />
      **Content:**
    ```json
      {      
          "message": "The name field is required. (and 3 more errors)",
          "errors": {
              "name": [
                "The name field is required."
              ],
              "rating": [
                "The rating field is required."
              ],
              "review": [
                "The review field is required."
              ],
              "book_id": [
                "The book id field is required."
              ]
          }
      }
    ```

### Return all genres

* **URL**

  /api/genres

* **Method:**

  `GET`

* **URL Params**

  **Required:**

  There are no required URL params

  **Optional:**

  There are no optional URL params

  **Example:**

  `/api/genres`

* **Success Response:**

    * **Code:** 200 <br />
      **Content:** <br />

```json
{
    "data": [
        {
            "id": 1,
            "name": "Action"
        }
    ],
    "message": "Genres retrieved"
}
```

* **Error Response:**

    * **Code:** 500 INTERNAL SERVER ERROR <br />
      **Content:** `{"message": "Unexpected error occurred"}`
