FORMAT: 1A
HOST: https://linc.semantic.md

# LINC API

LINC is a simple API allowing Lion Guardians to identify lions using computer vision.

# LINC API Root [/]

## Group Authentication

The CV Server implements basic authentication. 

## Group Lion

Resources related to lions in the API.

## Lion [/lions/{lion_id}]

+ Parameters
    + lion_id: 2 (required, numeric) - ID of the lion in form of a numeric

+ Attributes
    + name: "masusu" (required, string) - name of the lion
    + organization_id: "2" (required, string) - id of managing organization
    + primary_image_set_id: "2" (required, string) - id of image set

### View a Lion Detail [GET]

+ Response 200 (application/json)

        {
        "id":2,
        "name":"Lauren",
        "primary_image_set_id":2,
        "organization_id":2,
        "_embedded":
            {
                "image_sets":
                    [
                        {
                            "id":2,
                            "is_verified":false,
                            "latitude":"-2.6527",
                            "longitude":"37.26058",
                            "gender":"female",
                            "date_of_birth":"2005-05-31T16:17:56.817Z",
                            "main_image_id":6,
                            "uploading_organization_id":2,
                            "notes":null,
                            "organization_id":2,
                            "user_id":2,
                            "has_cv_results":false,
                            "has_cv_request":false,
                            "_embedded":
                                {
                                    "images":
                                        [
                                            {
                                                "id":4,
                                                "image_type":"cv",
                                                "is_public":true,
                                                "thumbnail_url":"http://lion-guardians-production.s3.amazonaws.com/2015/05/31/16/17/59/133/uploads_2Fe3f10f18_d176_41a7_84b9_0b2c5fef81e7_2F38yrs_Sikiria_300x300.jpg",
                                                "main_url":"http://lion-guardians-production.s3.amazonaws.com/2015/05/31/16/18/00/752/uploads_2Fe3f10f18_d176_41a7_84b9_0b2c5fef81e7_2F38yrs_Sikiria_300x300.jpg",
                                                "url":"http://lion-guardians-production.s3.amazonaws.com/2015/05/31/16/17/59/608/uploads_2Fe3f10f18_d176_41a7_84b9_0b2c5fef81e7_2F38yrs_Sikiria_300x300.jpg"},
                                                {
                                                    "id":6,
                                                    "image_type":"markings",
                                                    "is_public":true,
                                                    "thumbnail_url":"http://lion-guardians-production.s3.amazonaws.com/2015/05/31/16/17/58/984/uploads_2F7d89e575_29b0_4012_85b0_280ae741e620_2FLion_waiting_in_Namibia.jpg",
                                                    "main_url":"http://lion-guardians-production.s3.amazonaws.com/2015/05/31/16/17/59/923/uploads_2F7d89e575_29b0_4012_85b0_280ae741e620_2FLion_waiting_in_Namibia.jpg",
                                                    "url":"http://lion-guardians-production.s3.amazonaws.com/2015/05/31/16/17/58/141/uploads_2F7d89e575_29b0_4012_85b0_280ae741e620_2FLion_waiting_in_Namibia.jpg"},
                                                    {
                                                        "id":5,
                                                        "image_type":"whisker",
                                                        "is_public":true,
                                                        "thumbnail_url":"http://lion-guardians-production.s3.amazonaws.com/2015/05/31/16/17/59/384/uploads_2Fc949fccd_1784_42bd_8ce4_99035c887874_2F58yrs_male_Sikiria.jpg",
                                                        "main_url":"http://lion-guardians-production.s3.amazonaws.com/2015/05/31/16/17/59/859/uploads_2Fc949fccd_1784_42bd_8ce4_99035c887874_2F58yrs_male_Sikiria.jpg",
                                                        "url":"http://lion-guardians-production.s3.amazonaws.com/2015/05/31/16/17/59/285/uploads_2Fc949fccd_1784_42bd_8ce4_99035c887874_2F58yrs_male_Sikiria.jpg"}]},
                                                        "lion_id":2
                                                    }
                                                ]
                                            }
                                        }

## Find Lions [/lions{?gender}{?name}{?organization_id}{?dob_start}{?dob_end}{?no_images}]

Get a list of matching lions with metadata information and associated image sets

+ Parameters
    + gender: "m" (optional, string) - gender of the lion in form of a string
    + organization_id: "20150507" (optional, alphanumeric) - ID of the organization in form of an alphanumeric string
    + dob_start: "YYYY-MM-DD" (optional, string) - start date for lion's birthdate range in YYYY-MM-DD format
    + dob_end: "YYYY-MM-DD" (optional, string) - end date for lion's birthdate range in YYYY-MM-DD format
    +no_images: “true” (optional, string) - used to return all lions and information but without their image uri’s to reduce bandwidth.

## Group Classifier

## Identification [/identifications]

Classify a set of image and return the predicted labels and associated confidence.

### Identification [POST]

+ Request (application/json)

        {
          "identification": {
            "images": [
              {"id": "3", "type": "markings", "url": "https://lg-201-created-development.s3.amazonaws.com/uploads%2F7d89e575-29b0-4012-85b0-280ae741e620%2FLion_waiting_in_Namibia.jpg"}
              ],
            "gender": "male",
            "age": "14",
            "lions": [
              {"id": "2", "url": "http://localhost:3000/lions/2", "updated_at": "2015-02-19 16:12:04 UTC"}
            ]
          }
        }
        
+ Response 200 (application/json)

        {
           "identification": {
             "id": "1234-5678-9012",
             "status": "queued",
             "lions": []
           }
        }

## Identification Status [/results/<job_id>]

Get status of identification task. The valid values of "status" are TBD but will likely be "queued", "processing", "finished", and "error".

+ Parameters
    + job_id: "e3464fc5-54d5-48db-bc05-761cad999cd8" (required, alphanumeric) - ID of the recognition job in form of an alphanumeric string

### Identification Status [GET]

+ Response 200 (application/json)

        {
          identification: {
            id: "e3464fc5-54d5-48db-bc05-761cad999cd8",
            status: "finished",
            lions: [                           #  Each lion object in the array will include an id and confidence 
                                                         # (in the range 0-1.0)
              {id: 35, confidence: 0.8},
              {id: 23, confidence: 0.6}
            ]
          }
        }


## Training [/train/]

Train a classifier for each lion and return the lion ids and associated classifier ids.

### Train Model [POST]

Note that if "tag_list" is included then only the classifiers for the specific lion ids are trained.

+ Request (multipart/form-data)

        + tag_list: ['1','2','20'] (optional, string array) - list of all tags to compare against in form of string array
        + force_train: 'y' (optional, string) - all classifiers are retrained

+ Response 200 (application/json)

        {
             "project_id": "562d70f614bdf74d153a5cf5",
             "status": "processing"
        }

### Training Status [GET]

Get status of training task. The valid values of "status" are TBD but will likely be "queued", "processing", "finished", and "error".
Note that if a classifier was not trained or there was an error training a classifier, then an empty string will be returned for the
 classifier id.

+ Response 200 (application/json)

        {
            "project_id": "562d70f614bdf74d153a5cf5",
            status: "finished",
            tag2model: {
                '1': '20150818-225816-066b',
                '2': '20130818-215216-066b',
                '3': ''
            }
        }

## Group Evaluation

## Accuracy [/accuracy/<lion_id>]

Return the accuracy for classifying images associated with lion_id.

### Accuracy [GET]

+ Response 200 (application/json)

        {
            "tag_id": "2",
            "model_id": "20151025-171816-e4b1",
            "accuracy": 95.0
        }

## Group Terms of Use

By using our API you are agreeing to our terms of use. You can read it here.
