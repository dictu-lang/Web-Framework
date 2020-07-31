# Dictu Web Framework

This project is a proof of concept project that essentially creates a very simple web framework in [Dictu](https://dictu-lang.com). To run this project you must have at least [Dictu version 0.9.0](https://github.com/dictu-lang/Dictu/releases/tag/v0.9.0) as this is when Dictu introduced the Socket module.

For installing Dictu, please see the [repository here](https://github.com/dictu-lang/Dictu).

## Getting started

Once you have Dictu installed you're ready to create your first website!

### Routing

The routes for the framework are defined in `routes.du`. In here we have a dictionary which maps endpoints and given HTTP verbs to an eventual function call. The outer key is the route coming in from the request, and an inner dictionary contains the HTTP verb for the request, this allows us to have the same route, yet act on many different HTTP verbs - GET, POST, etc.

```js
import JSON;

import "response.du" as ResponseModule;
import "controllers/indexController.du" as IndexControllerModule;


var routes = {
    "/": {
        "GET": IndexControllerModule.index,
    },
    "/json": {
        "GET": def () => {
            return ResponseModule.JsonResponse(['test']);
        }
    }
};
```

[Base routes.du file]()

### Controllers

The controllers directory allows you to create controller files which can handle your business logic for your website. The project comes with an example controller `indexController.du` within the directory with the following content.

```js
import "utils/utils.du" as UtilModule;

def index() {
    return UtilModule.view('index');
}
```

Within this file, we can create as many functions as we need to handle the different endpoints all relating to similar logic, for example, the above could introduce a new function to handle `POST` HTTP requests. You can also create more controller files to handle other routes and import to `routes.du` as required.

### Responses

The return value from a route must be a response object. There are currently two built in response objects.

```js
class Response < BaseResponse {
    init(content, mimeType="text/html", status=200) {
        this.content = content;
        this.contentLength = content.len();
        this.mimeType = mimeType;
        this.status = this.statusCodes[status];
    }
}

class JsonResponse < BaseResponse {
    init(content, status="200 OK") {
        this.content = JSON.stringify(content);
        this.contentLength = this.content.len();
        this.mimeType = "application/json";
        this.status = this.statusCodes[status];
    }
}
```

The first is a generic `Reponse` object which will mainly be used for serving HTML content, however it does allow for the contents mime type to be changed at will. The second is used for JSON responses, and will automatically stringify the content passed to it, allowing things like arrays be passed to a JSON object and be handled accordingly.

#### Example usage

There is an example of using the `JsonResponse` within the `routes.du` file already.

```js
"/json": {
	"GET": def () => {
		return ResponseModule.JsonResponse(['test']);
	}
}
```

### Views

Views are HTML files that will be sent to the user. Within this framework they live within the `views` directory. When you wish to return a view to a user from a given route, you can use the `utils.du`'s `view` helper function.

#### Example

There is an example of using the `view()` helper function within the `indexController.du` file already.

```js
import "utils/utils.du" as UtilModule;

def index() {
    return UtilModule.view('index');
}
```

As you can see, when using the helper function the `.html` extension should not be passed, and only the filename.

## Contributing

This is currently a very minimalistic framework and is missing quite a lot of functionality. Contributions to this framework are however incredibly welcome either through issue reports or pull requests!

## License

This project is licensed under the MIT license.