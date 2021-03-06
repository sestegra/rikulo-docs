#Form Handling

When handling the data of a submitted form, you can use [ObjectUtil.inject()](commons:mirrors) to convert the query parameters into an object.

For example, assume we have a form as follows:

    <form action="search">
      <table>
        <tr>
          <td colspan="2">Search</td>
        </tr>
        <tr>
          <td colspan="2"><input name="text" autofocus="true" size="60"/></td>
        </tr>
        <tr>
          <td>Since</td>
          <td><input name="since" type="date"/></td>
        </tr>
        <tr>
          <td>Within</td>
          <td><input name="within" type="number"/> days</td>
        </tr>
        <tr>
          <td colspan="2"><input type="checkbox" name="hasAttachment" id="hatt"/>
            <label for="hatt">Has attachment</label></td>
        </tr>
        <tr>
          <td colspan="2" align="right"><input type="submit"></td>
        </tr>
      </table>
    </form>

Then, you can implement a class called `Criteria` to hold the information as follows:

    class Criteria {
      String text = "";
      DateTime since;
      int within;
      bool hasAttachment = false;
    }

Then, you can implement a request handling for the action called `search` in the above example as follows:

    Future search(HttpConnect connect) {
      final criteria = new Criteria();
      ObjectUtil.inject(criteria, connect.request.queryParameters, silent: true);
      return searchResult(connect, criteria: criteria);
    }

> For a runnable example, you can refer to the [features](source:test) example.

> For more complicated examples of injection, you can refer to [these examples](https://github.com/rikulo/commons/blob/master/test/inject.dart).

##Custom coercion

By default, [ObjectUtil.inject()](commons:mirrors) will coerce the basic types, such as `int`, `num`, `double`, `String`, `bool`, `DateTime`, and [Color](commons:util) automatically. If you need to coerce the custom types, you can implement the coercion and pass to the `coerce` parameter.

##Validation

You can validate the values in a closure by passing it to the `validate` parameter when calling [ObjectUtil.inject()](commons:mirrors).

> Notice that the validation is called after the value is coerced successfully, and before it is assigned to the target object.

##Handle POST requests

Notice that [HttpRequest.queryParameters](dart:io) includes only the parameters found in the query string. In other words, if the method of the form is `POST` (i.e., `<form method="POST"...`), you have to parse the body of the POST request. It can be done easily by use of [HttpUtil.decodePostedParameters](commons:io):

      HttpUtil.decodePostedParameters(request, request.queryParameters)
      .then((Map<String, String> params) {
        //handle params
      });

> If the body is a JSON string, you can use [IOUtil.readAsJson](commons:io) instead.
