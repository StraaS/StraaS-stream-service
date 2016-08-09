Control APIs
==============

[Add Job](#add_job)

<a name="add_job"></a>**Add Job**
----
  Returns json data about a single user.

* **URL**

  /jobs

* **Method:**

  `POST`

*  **URL Params**

* **Data Params**

  Required:

  Optional:

  encoderProfile=[JSON body]
  ```json
  {
    "encoderProfile": [
      {
        "resolution": "1080p",
        "bitrate": 5000
      },
      {
        "resolution": "720p",
        "bitrate": 2000
      },
      {
        "resolution": "360p",
        "bitrate": 1000
      }
    ]
  }
  ```

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ id : 12, name : "Michael Bloom" }`

* **Error Response:**

  * **Code:** 404 NOT FOUND <br />
    **Content:** `{ error : "User doesn't exist" }`

  OR

  * **Code:** 401 UNAUTHORIZED <br />
    **Content:** `{ error : "You are unauthorized to make this request." }`

* **Sample Call:**

  ```shell
  curl -X POST -d '{"encoderProfile":[{"resolution":"720p","bitrate":2000}]}' "https://tx.straas.net/<account_id>/api/v1/jobs"
  ```
