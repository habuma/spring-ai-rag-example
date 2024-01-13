Spring in Action AI
---
Simple example to load the entire text of Spring in Action (or any other PDF)
into a vector store and then expose an API through which questions can be
asked about the book's content.

Before running the application, you'll need to acquire an OpenAI API key.
Set the API key as an environment variable named `OPENAI_API_KEY`. E.g.,

```
$ export OPENAI_API_KEY=sk-1234567890abcdef1234567890abcdef
```

You'll also need a PDF for it to load. The code has been written to load
a file named "Spring_in_Action_Sixth_Edition.pdf" from src/main/resources/data.
For what may or may not be obvious reasons, I've chosen not to include that PDF
in this repository, so you'll need to obtain your own copy. Of course, most any
other PDF will also work. You'll just need to make sure that the `app.pdf.name`
property is set to reference your PDF's filename.

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

Or with HTTPie it's a little easier:

```
http :8080/ask question="What annotation should I use to create a REST controller?"
```

Note that the questions will be answered using the content of the book.
That means that sometimes the answers will be given in the context of the
book's example application, Taco Cloud.
