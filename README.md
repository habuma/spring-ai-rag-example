Spring AI RAG Example
---
Simple example to load the entire text of a PDF  into a vector store and 
then expose an API through which questions can be asked about the PDF's 
content.

Before running the application, you'll need to acquire an OpenAI API key.
Set the API key as an environment variable named `OPENAI_API_KEY`. E.g.,

```
$ export OPENAI_API_KEY=sk-1234567890abcdef1234567890abcdef
```

You'll also need a PDF for it to load. Place the PDF in src/main/resources/data
and set the `app.pdf.name` property in src/main/resources/application.properties
to the name of the PDF.

Then run the application as you would any Spring Boot application. For
example, using Maven:

```
$ mvn spring-boot:run
```

The first time you run it, it will take a little while to load the PDF into
the vector store (which will be persisted at /tmp/vectorstore.json). Subsequent
runs will just use the persisted vector store and not try to load the PDF again.

Then you can use `curl` to ask questions:

```
$ curl localhost:8080/ask -H"Content-type: application/json" -d '{"question": "What annotation should I use to create a REST controller?"}'
```

> The question shown in the example was used to ask questions against my book,
[Spring in Action, 6th Edition](https://www.manning.com/books/spring-in-action-sixth-edition?a_aid=habuma&a_bid=f205d999&chan=habuma). 
You'll want to ask questions relevant to whatever PDF you're using.

Or with HTTPie it's a little easier:

```
http :8080/ask question="What annotation should I use to create a REST controller?"
```

