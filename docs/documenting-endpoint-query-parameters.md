# Documenting parameters for an endpoint

## Specifying query parameters
To describe query parameters for your endpoint, use the `@queryParam` annotation.

The `@queryParam` annotation takes the name of the parameter, an optional "required" label, and a description.

Examples:

```php
/**
 * @queryParam sort Field to sort by. Defaults to 'id'.
 * @queryParam fields required Comma-separated fields to include in the response
 * @queryParam filters[published_at] Filter by date published.
 * @queryParam filters[title] Filter by title.
 */
public function listPosts()
{
    // ...
}
```

They will be included in the generated documentation text and example requests:

![](images/endpoint-queryparams-1.png)

![](images/endpoint-queryparams-2.png)


If you're using a FormRequwst in your controller, you can also add the `@queryParam` annotation there instead, and Scribe will fetch it.

```php
/**
 * @queryParam user_id required The id of the user. Example: me
 */
class MyRequest extends \Illuminate\Foundation\Http\FormRequest
{

}

// in your controller...
public function createPost(MyRequest $request)
{
    // ...
}
```

## Specifying example values
By default, Scribe will generate a random value for each parameter to be used in the example requests and response calls. If you'd like to use a specific example value, you can do so by adding `Example: your-example` to the end of your description.

You can also exclude a particular parameter from the generated examples by ending with `No-example` instead. This will also prevent the parameter from being sent along in response calls.

For instance:

```php
    /**
     * @queryParam sort Field to sort by. Defaults to 'id'. Example: published_at
     * @queryParam fields required Comma-separated fields to include in the response. Example: title,published_at,id
     * @queryParam filters[published_at] Filter by date published. No-example
     * @queryParam filters[title] Filter by title. No-example
    */
```

![](images/endpoint-queryparams-3.png)

## Describing URL parameters
To describe parameters in the URL, use the `@urlParam` annotation. For instance, if you defined your Laravel route like this:

```php
Route::get("/post/{id}/{lang?}")
```

you can use this annotation to describe the `id` and `lang` parameters as shown below. The annotation takes the name of the parameter, an optional "required" label, and then its description. Like with `@queryParams`, a random value will be generated, but you can specify the value to be used in examples and response calls using the `Example: ` syntax.

```php
/**
 * @urlParam id required The ID of the post.
 * @urlParam lang The language. Example: en
 */
public function getPost()
{
    // ...
}
```

![](images/endpoint-urlparams-1.png)

 If you specify `No-example` for a parameter that's optional (`lang` in our example), Scribe will omit that parameter in requests and response calls.

```php
/**
 * @urlParam id required The ID of the post.
 * @urlParam lang The language. No-example
 */
public function getPost()
{
    // ...
}
```

![](images/endpoint-urlparams-2.png)