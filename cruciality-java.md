Arranging Cruciality Requests
------------------------------

Our system processes different kinds of information for our customers. One of our analyses, called "cruciality analysis", 
runs based on a set of companies provided as a parameter. Our back-end application receives this information and sends a 
final request to our processing algorithm. The time it takes for our cruciality endpoint to process the requests is 
logaritmic - *log2(n)*, where **n** is the amount of companies provided by the customer. Because of this we'd like to 
prioritise processing requests where the number of companies is larger. Your task is to help us organise the way we send 
requests to our back-end processing algorithm endpoint and then send this ordered requests to our cruciality endpoint.

Lately our application has grown in popularity. Ranking 1st in our field we have millions of customers sending their 
cruciality requests. Because of this you should know that picking a good solution to keep the data sorted is crucial for our
task. You could think that for the example data a list is enough, but you need to make it usable for cases where the amount
of requests in line is constantly growing and being updated.

In order to implement your task you need to create a Java application where you will handle the given information, arrange 
it and finally send it to the cruciality endpoint.

To simplify this task you won't have to write the code to process the http requests coming from the user interface, instead 
you can use the following JSON objects as an example of the information that you'll need to arrange for this task.

    var request1 = """
    {
      "user":"fryderyk.chopin@example.com",
      "analysis-type":"essentiality",
      "companies":["Yamaha", "Sennheiser", "Shure", "Roland", "Fender"]
    }
    """;

    var request2 = """
    {
      "user":"karl.marx@example.com",
      "analysis-type":"essentiality",
      "companies":["Lada", "Tetris", "Ural"]
    }
    """;

    var request3 = """
    {
      "user":"salma.hayek@example.com",
      "analysis-type":"essentiality",
      "companies":["Apple", "Audi"]
    }
    """;

    var request4 = """
    {
      "user":"vincent.van.gogh@example.com",
      "analysis-type":"essentiality",
      "companies":["Faber-Castell"]
    }
    """;

    var request5 = """
    {
      "user":"dolores.abernathy@example.com",
      "analysis-type":"essentiality",
      "companies":["OpenAI", "Tesla", "Google", "Sony", "Nvidia", "Amazon", "IBM"]
    }
    """;

    var request6 = """
    {
      "user":"ludwig.von.bertalanffy@example.com",
      "analysis-type":"essentiality",
      "companies":["Patagonia", "Beyond Meat"]
    }
    """;


If you consider it useful, in order to parse these strings you can use a library like GSon. The code below is an example 
of how to parse a json string using this library (feel free to use your own method if you don't like this approach ;) ).

    MyRequestClass request = new GsonBuilder()
       .create()
       .fromJson(request1, MyRequestClass.class);

The Cruciality endpoint that you need to use accepts POST requests in the following URL 
https://europe-west3-test-iplytics.cloudfunctions.net/process-companies 
An example of the expected request body is found below.

    {
      "id": 1,
      "user": "salma.hayek@example.com",
      "companies":["Apple", "Audi"]
    }

Remember this is a pair-progaming exercise, feel free to ask your peer developers for help if you need. Also feel free to 
use any web resource or additional library. We think you won't need many additional tools besides the standard java libraries 
(and GSon if you want to use it) but you can go as fancy as you want. Also remember to keep an eye on the time limit, you 
will have 1 hour to work on our very important and crucial cruciality arranging project. Good luck!
