## Devices

#### **GET** /device_list

response:

    {
        message: 'OK',
        data: {
            chipGroup:[{
                id: String|Number,
                name: String,
                chips: [{
                    id: String|Number,
                    mac: String,
                    name: String,
                    last_seen: String|Date,
                    visible: Boolean
                }]
            }]
        }
    }

#### **POST** /register_chips

params mime: application/json

body:

| key      | value                          | desc |
| -------- | ------------------------------ | ---- |
| chip_ids | `Array` (exp. [0, 1, 2, 3, 4]) | \*   |

exp:

    {"chip_ids":[0,1,2,3]}

response:

    {
    	message: 'Successful',
    	data: {}
    }

#### **POST** /deregister_chips

params mime: application/json

body:

| key      | value                          | desc |
| -------- | ------------------------------ | ---- |
| chip_ids | `Array` (exp. [0, 1, 2, 3, 4]) | \*   |

exp:

    {"chip_ids":[0,1,2,3]}

response:

    {
    	message: 'Successful',
    	data: {}
    }

## Creating Commands

#### **GET** /command_list

response:

    {
    	message: 'OK',
    	data: {
            normal: [{
                id: String|Number,
                name: String,
                tag: String,
                created_time: String|Date,
                schedule: Boolean(false),
                info: {
                    command_prefix: String,
                    f: String,
                    p: String,
                    n: String,
                }
            }],
            scheduled: [{
                id: String|Number,
                name: String,
                tag: String,
                created_time: String|Date,
                schedule: {
                    start_time: String|Date,
                    periodic: Boolean,
                    end_time: String|Date,
                    repeat_period: String
                },
                info: {
                    command_prefix: String,
                    f: String,
                    p: String,
                    n: String,
                }
            }]
        }
    }

#### **POST** /command

params mime: application/json

body:

| key      | value                            | desc |
| -------- | -------------------------------- | ---- |
| tag      | `String` ('LED'/'Heater'/'Misc') | \*   |
| name     | `String`                         | \*   |
| command  | `String` (exp. 'aF10fP10pN10n')  | \*   |
| isActive | `Boolean`                        | \*   |
| schedule | `Object` `Boolean(false)`        | \*   |

`schedule` object format:

| key           | value                       | desc                              |
| ------------- | --------------------------- | --------------------------------- |
| start_date    | `String` (YYYY-MM-DD HH:MM) | \*                                |
| end_date      | `String` (YYYY-MM-DD HH:MM) | \*                                |
| periodic      | `Boolean`                   | \*                                |
| repeat_period | `Number`                    | Optional if `periodic` is `false` |
| unit          | `String`                    | Optional if `periodic` is `false` |

exp:

    {"tag":"Misc","name":"testCommand","command":"s","isActive":"true", "schedule":{"start_date":"2018-03-09 15:44","end_date":""2018-03-09 15:44", "periodic":true, "repeat_period": 3, "unit":"d"}}

response:

    {
    	message: 'Successful',
    	data: {
            id: String|Number,
            name: String,
    		tag: String
    	}
    }

#### **PUT** /command/:id

params mime: application/json

body: like `**POST** /command` only with values allowed to be **optional**

response:

    {
        message: 'Successful',
        data: {
            id: String|Number,
            name: String,
            tag: String
        }
    }

#### **DELETE** /command/:id

response:

    {
    	message: 'Successful',
    	data: {}
    }

## Sending Commands

#### **POST** /send_command

params mime: application/json

body:

| key          | value   | desc |
| ------------ | ------- | ---- |
| command_data | `Array` | \*   |

`command_data` array item format:

| key        | value                    | desc |
| ---------- | ------------------------ | ---- |
| device_id  | `String`                 | \*   |
| command_id | `Array` (exp. [0,1,2,3]) | \*   |

exp:

    {"command_data":[{"device_id":"1011", "command_id":[0,1,2,3]}]}

response:

    {
    	message: 'Successful',
    	data: {}
    }

## Admin

#### **GET** /user_list

response:

    {
    	message: 'Successful',
    	data: [{
    		id: String|Number,
    		account: String,
    		name: String,
    	}]
    }

#### **POST** /user

params mime: application/json

body:

| key      | value                            | desc |
| -------- | -------------------------------- | ---- |
| account  | `String` (exp. 'test@gmail.com') | \*   |
| password | `String`                         | \*   |
| name     | `String`                         | \*   |

exp:

    {"account":"test@gmail.com","password":"test","name":"test"}

response:

    {
        message: 'Successful',
        data: {
            account: String,
            name: String,
            id: String|Number
        }
    }

#### **PUT** /user/:id

params mime: application/json

body:

| key      | value    | desc     |
| -------- | -------- | -------- |
| password | `String` | optional |
| name     | `String` | optional |

exp:

    {"password":"test","name":"test"}

response:

    {
    	message: 'Successful',
    	data: {
    		account: String,
    		name: String,
    		id: String|Number
    	}
    }

#### **DELETE** /user/:id

response:

    {
    	message: 'Successful',
    	data: {}
    }

## Log

#### **GET** /log

response:

    {
        message: 'Successful',
        data: [{
            chip_name: String,
            command_name: String,
            command_command: String,
            send_date: String|Date,
            scheduled: String,
            error: String,
            f: Integer,
            p: Integer,
            n: Integer
        }]
    }
