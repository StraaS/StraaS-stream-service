Control APIs
==============

Default API response: http code 200

[Add Job](#add_job)

<a name="add_job"></a>**Add Job**
----
Add a job to start batch file transcoding (Each single file processing is called a `task`; One `job` could include many tasks). The registered`callback_url` will be notified when defined event occurs.

* **URL**

  POST `/jobs`

* **Request Body**

  | Field | Data Type | Required | Description | Remark |
  | --- | --- | --- | --- | --- |
  | `account` | string | yes | | |
  | `job_id` | string | yes | the unique id (**you should generate**) to identify this job | `^[A-Za-z0-9_.]+$`; `length <= 16` |
  | `callback_url` | string | | the callback entry (`POST` method) for this job | ex: `http://www.abc.com/notifications`, detail [here](#callback) |
  | `encoder_profile` | JSON | | customized bitrate for some resolution | 3 supporting resolutions: `1080p`, `720p`, `360p`; bitrate unit (k bits / sec); example as [here](#encoder_profile) |

* **Response**

  Default API response

* **Error Response**

  | Code | Type | Description |
  | --- | --- | --- |
  | 400 | Bad request | |
  | 401 | Unauthorized | |
  | 500 | Internal server error | |

* **Example:**

  ```go
  // request
  curl -X POST \
    --header 'Authorization: <YOUR_APPLICATION_TOKEN>' \
    -d '{"account":"f695cb7452d153e985f3dafa7c026f91", \
    "job_id":"abcd", \
    "callback_url":"straas.io/callback", \
    "encoder_profile":[{"resolution":"720p","bitrate":2000}]}' \
    "https://tx.straas.io/api/v1/jobs"

  // response
  200
  ```

* <a name="encoder_profile"></a>**Encoder Profile**

  ```go
  {
    "encoder_profile": [
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

* <a name="callback"></a>**Callback**

  The `callback_url` passed in for this batch job will be notified when defined events occur.
  The registered callback URL should be a `POST` HTTP entry. The relating information about the job will
  be contained in the HTTP request body (JSON body) as below format.

  | CB event | meaning | remark |
  | --- | --- | --- |
  | `preprocess` | move files to cloud storage | |
  | `process` | start processing the job | |
  | `transcoded` | one task of the job has completed | |
  | `postprocess` | fetch result from cloud storage | |
  | `completed` | all tasks of the job have completed | |


  ```go
  // event: preprocess
  {
	"job_id": "abc",
    "event": "preprocess"
  }
  ```

  ```go
  // event: process
  {
	"job_id": "abc",
    "event": "process"
  }
  ```

  ```go
  // event: transcoded (will be triggered by each transcoding task finished in the batch job)
  {
	"job_id": "abc",
    "event": "transcoded",
    "file": "test-1.mov",
    "status": "completed" // {completed, error}
  }
  ```

  ```go
  // event: postprocess
  {
	"job_id": "abc",
    "event": "postprocess"
  }
  ```

  ```go
  // event: completed
  {
	"job_id": "abc",
    "event": "completed"
  }
  ```
