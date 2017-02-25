source: response.py

\# Responses

&gt; 与基本的HttpResponse对象不同, TemplateResponse 对象保留view提供的上下文的详细信息以计算 response.  Response的最终输出直到它在稍后的响应过程中被需要才会计算。

&gt;

&gt; — \[Django 文档\]\[cite\]

REST framework 通过提供一个\`Response\`类来支持 HTTP content negotiation，该类允许你返回可以呈现为多种内容类型的内容，具体取决于客户端的请求。

\`Response\` 类的子类 Django的 \`SimpleTemplateResponse\`。  \`Response\` 对象用Python基本数据类型初始化。 然后REST framework 使用标准的HTTP content negotiation 来确定如何呈现最终的相应内容。

你并不需要一定是用 \`Response\` 类， 你可以从你的视图返回常规的 \`HttpResponse\` 或者 \`StreamingHttpResponse\` 对象。  使用 \`Response\` 类只提供了一个可以呈现多种格式的更好的界面来返回 content-negotiated 的 Web API 响应。

除非由于某种原因你要对 REST framework 做大量的自定义，否则你应该始终对返回对象的views使用 \`APIView\` 类或者\`@api\_view\` 函数。这样做可以确保视图在返回之前能够执行 content negotiation 并且为响应选择适当的渲染器。

---

\# 创建 responses

\#\# Response\(\)

\*\*Signature:\*\* \`Response\(data, status=None, template\_name=None, headers=None, content\_type=None\)\`

与常规的 \`HttpResponse\` 对象不同，你不能使用渲染内容来实例化一个 \`Response\` 对象，而是传递未渲染的数据，包含任何Python基本数据类型。

\`Response\` 类使用的渲染器无法自行处理想Django model实例这样的复杂数据类型，因此你需要在创建\`Response\`对象之前将数据序列化为基本数据类型。

你可以使用 REST framework的\`Serializer\` 类来执行此类数据的序列化，或者使用你自定义的来序列化。

参数:

\* \`data\`: response的数列化数据.

\* \`status\`:  response的状态码。默认是200.  另行参阅 \[status codes\]\[statuscodes\].

\* \`template\_name\`: \`HTMLRenderer\` 选择要使用的模板名称。

\* \`headers\`: A dictionary of HTTP headers to use in the response.

\* \`content\_type\`: response的内容类型。通常由渲染器自行设置，由content negotiation确定，但是在某些情况下，你需要明确指定内容类型。

---

\# 属性

\#\# .data

\`Request\` 对象的未渲染内容。

\#\# .status\_code

HTTP 响应的数字状态吗。

\#\# .content

response的呈现内容。 \`.render\(\)\` 方法必须先调用才能访问\`.content\` 。

\#\# .template\_name

\`template\_name\` 只有在使用 \`HTMLRenderer\` 或者其他自定义模板作为response的渲染器时才需要提供该属性。

\#\# .accepted\_renderer

将用于呈现response的render实例。

自动通过 \`APIView\` 或者 \`@api\_view\` 在view返回response之前设置。

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

