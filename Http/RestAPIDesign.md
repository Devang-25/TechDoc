# RestAPI Design

1. __API Keyword__
+ API keyword is better to be included in the URL. It emphasizes this URL is for the API, not the webside.
```Http
https://www.app.com/api
```
2. __Version__
+ Due to we can't force version upgrade, we should give our api a version. Then, we can set version support policy, to tell user till when we will support this api. Version should be:
> * Positive
> * No decimal
> * Could be prefixed with v
+ Version can be put into:
> * URL path (The easiest way to implement)
> * URL parameter (Do not use)
> * Header (It is the most correct form)
```Http
https://www.app.com/api/v1
```

3. __Entity__
+ REST deals with resources, or entities, so it should be one word, not a verb, this is the HTTP verb's work.
```Http
https://www.app.com/api/v1/order
https://www.app.com/api/v2/employees
```

4. __Entity ID__
+ ID is part of the URL
```Http
https://www.app.com/api/v1/order/1
```
