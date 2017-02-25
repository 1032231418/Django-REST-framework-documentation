source: response.py

\# Responses

&gt; 与基本的HttpResponse对象不同, TemplateResponse 对象保留view提供的上下文的详细信息以计算 response.  Response的最终输出直到它在稍后的响应过程中被需要才会计算。

&gt;

&gt; — \[Django 文档\]\[cite\]

REST framework 通过提供一个\`Response\`类来支持 HTTP content negotiation，该类允许你返回可以呈现为多种内容类型的内容，具体取决于客户端的请求。

 \`Response\` 类的子类 Django的 \`SimpleTemplateResponse\`。  \`Response\` 对象用Python原生的数据类型初始化。 然后REST framework 使用标准的HTTP content negotiation 来确定如何呈现最终的相应内容。

你并不需要一定是用 \`Response\` 类， 你可以从你的视图返回常规的 \`HttpResponse\` 或者 \`StreamingHttpResponse\` 对象。  使用 \`Response\` 类只提供了一个更好的界面来返回 content-negotiated 的 Web API 响应， 可以呈现多种格式。

Unless you want to heavily customize REST framework for some reason, you should always use an \`APIView\` class or \`@api\_view\` function for views that return \`Response\` objects.  Doing so ensures that the view can perform content negotiation and select the appropriate renderer for the response, before it is returned from the view.

---

\# Creating responses

\#\# Response\(\)

\*\*Signature:\*\* \`Response\(data, status=None, template\_name=None, headers=None, content\_type=None\)\`

Unlike regular \`HttpResponse\` objects, you do not instantiate \`Response\` objects with rendered content.  Instead you pass in unrendered data, which may consist of any Python primitives.

The renderers used by the \`Response\` class cannot natively handle complex datatypes such as Django model instances, so you need to serialize the data into primitive datatypes before creating the \`Response\` object.

You can use REST framework's \`Serializer\` classes to perform this data serialization, or use your own custom serialization.

Arguments:

\* \`data\`: The serialized data for the response.

\* \`status\`: A status code for the response.  Defaults to 200.  See also \[status codes\]\[statuscodes\].

\* \`template\_name\`: A template name to use if \`HTMLRenderer\` is selected.

\* \`headers\`: A dictionary of HTTP headers to use in the response.

\* \`content\_type\`: The content type of the response.  Typically, this will be set automatically by the renderer as determined by content negotiation, but there may be some cases where you need to specify the content type explicitly.

---

\# Attributes

\#\# .data

The unrendered content of a \`Request\` object.

\#\# .status\_code

The numeric status code of the HTTP response.

\#\# .content

The rendered content of the response.  The \`.render\(\)\` method must have been called before \`.content\` can be accessed.

\#\# .template\_name

The \`template\_name\`, if supplied.  Only required if \`HTMLRenderer\` or some other custom template renderer is the accepted renderer for the response.

\#\# .accepted\_renderer

The renderer instance that will be used to render the response.

Set automatically by the \`APIView\` or \`@api\_view\` immediately before the response is returned from the view.

\#\# .accepted\_media\_type

The media type that was selected by the content negotiation stage.

Set automatically by the \`APIView\` or \`@api\_view\` immediately before the response is returned from the view.

\#\# .renderer\_context

A dictionary of additional context information that will be passed to the renderer's \`.render\(\)\` method.

Set automatically by the \`APIView\` or \`@api\_view\` immediately before the response is returned from the view.

---

\# Standard HttpResponse attributes

The \`Response\` class extends \`SimpleTemplateResponse\`, and all the usual attributes and methods are also available on the response.  For example you can set headers on the response in the standard way:

```
response = Response\(\)

response\['Cache-Control'\] = 'no-cache'
```

\#\# .render\(\)

\*\*Signature:\*\* \`.render\(\)\`

As with any other \`TemplateResponse\`, this method is called to render the serialized data of the response into the final response content.  When \`.render\(\)\` is called, the response content will be set to the result of calling the \`.render\(data, accepted\_media\_type, renderer\_context\)\` method on the \`accepted\_renderer\` instance.

You won't typically need to call \`.render\(\)\` yourself, as it's handled by Django's standard response cycle.

\[cite\]: [https://docs.djangoproject.com/en/stable/stable/template-response/](https://docs.djangoproject.com/en/stable/stable/template-response/)

\[statuscodes\]: status-codes.md

