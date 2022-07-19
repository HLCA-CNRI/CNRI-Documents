# API 구조 - Lyllia

## auth

- path: "auth/users"

### user

> status
>
> - O : 사용
> - X : 미사용(불필요 등의 이유)

|      함수명      | API method |         path         | status |
| :--------------: | :--------: | :------------------: | :----: |
|      signUp      |    POST    |       "signup"       |   O    |
|    checkEmail    |    GET     | "check-email/:email" |   O    |
|      signIn      |    POST    |       "signin"       |   X    |
|     signOut      |    GET     |      "signout"       |   X    |
| sendTempPassword |    PUT     |  "password/:email"   |   O    |

## user

|     함수명     | API method |     path      | status |
| :------------: | :--------: | :-----------: | :----: |
|     getMe      |    GET     |     "me"      |   O    |
|    updateMe    |   PATCH    |     "me"      |   O    |
| updatePassword |    PUT     | "me/password" |   O    |
|    deleteMe    |   DELETE   |     "me"      |   O    |

## admin
