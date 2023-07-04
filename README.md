# Chat-Bot
 

Importing necessary modules:

Flask: A web framework for Python used to build web applications.
render_template: A function that allows rendering HTML templates.
request: A module that handles HTTP requests sent to the Flask application.
redirect: A function to redirect the user to a different route or URL.
url_for: A function used to generate URLs for Flask routes.
jsonify: A function to create a JSON response from a Python dictionary.
os: A module that provides a way to interact with the operating system.
Importing AIML and autocorrect modules:

aiml: A Python implementation of AIML (Artificial Intelligence Markup Language), which is used for creating chatbots and natural language processing.
autocorrect: A module that provides spelling correction functionality.
Downloading NLTK resources:

nltk.download('wordnet'): Downloads the WordNet corpus from NLTK (Natural Language Toolkit), which is a lexical database for the English language.
Initializing a lemmatizer:

lemmatizer = WordNetLemmatizer(): Creates an instance of the WordNetLemmatizer class from NLTK. Lemmatization is the process of reducing words to their base or root form.
Overall, this code sets up a Flask application and imports necessary modules for implementing a chatbot using AIML. It also downloads the WordNet corpus from NLTK and initializes a WordNet lemmatizer for word lemmatization.

 

This code snippet demonstrates the implementation of Neo4j using the Py2neo library within a Flask application. Here's a breakdown of the code:

Importing necessary modules:

Graph, Node, Relationship, NodeMatcher: These classes are provided by the Py2neo library for interacting with the Neo4j graph database.
Establishing a connection to the Neo4j database:

graph = Graph(password="123456789"): Creates an instance of the Graph class, which represents a connection to the Neo4j database. In this example, the password is provided for authentication.
Creating a Flask application:

app = Flask(__name__): Initializes a Flask application. The __name__ argument represents the name of the current module or package.
With this setup, you can use the graph object to perform various operations on the Neo4j database within your Flask application

 
This code snippet demonstrates the initialization and loading of an AIML (Artificial Intelligence Markup Language) brain file using the AIML Python library. Here's an explanation of the code:
Setting the path for the AIML brain file:
BRAIN_FILE = "./pretrained_model/aiml_pretrained_model.dump": Specifies the file path where the AIML brain file will be stored or loaded from.
Creating an AIML kernel:
k = aiml.Kernel(): Creates an instance of the AIML Kernel class, which is the main component responsible for processing AIML files and generating responses.
Checking if the brain file exists:

if os.path.exists(BRAIN_FILE):: Checks if the AIML brain file already exists.
Loading the brain file:
k.loadBrain(BRAIN_FILE): If the brain file exists, it is loaded into the AIML kernel using the loadBrain method. This restores the previous state of the kernel, including learned patterns and responses.
Parsing AIML files and bootstrap:
k.bootstrap(learnFiles="./pretrained_model/learningFileList.aiml", commands="load aiml"): If the brain file does not exist, the AIML kernel performs a bootstrap process. It parses the AIML files specified in learnFiles and executes the commands specified in commands. This allows the kernel to learn patterns and generate appropriate responses.
Saving the brain file:
k.saveBrain(BRAIN_FILE): After parsing AIML files or loading the brain file, the AIML kernel saves its state to the AIML brain file using the saveBrain method. This ensures that the learned patterns and responses are preserved for future use.
By following this code logic, the AIML kernel is initialized, and if a brain file exists, it is loaded. Otherwise, AIML files are parsed, and the learned patterns and responses are saved to the brain file for subsequent usage.
This code snippet defines a Flask route for processing a login request and interacting with a Neo4j graph database. Here's an explanation of the code:

Route definition:
@app.route('/process_login', methods=['POST']): Decorates the process_login function with a Flask route. This route is accessed through the URL '/process_login' and only allows POST requests.
Request processing:
user_details = request.json: Retrieves the JSON data sent in the request body and assigns it to the user_details variable.
username = user_details['username']: Extracts the 'username' field from the JSON data.
password = user_details['password']: Extracts the 'password' field from the JSON data.
Querying the Neo4j graph database:
node1 = graph.nodes.match("Person", name=username, password=password).first(): Uses the NodeMatcher class from Py2neo to query the graph database. It attempts to find a node labeled as "Person" with matching 'name' and 'password' properties. The .first() method retrieves the first matching node.
Handling the query result:
if node1:: Checks if a matching node was found in the graph database.
If true, it means the user exists and a successful login message is returned as a JSON response.
If false, it means the user does not exist or the password is incorrect. An error message is returned as a JSON response.
The code assumes that the graph object represents a connected instance of the Neo4j graph database, and the relevant Neo4j nodes are labeled as "Person" with 'name' and 'password' properties. Depending on the outcome of the login attempt, the appropriate response message is returned as a JSON object.

 

This code snippet defines a Flask route for processing a user registration request and interacting with a Neo4j graph database. Here's an explanation of the code:
Route definition:
@app.route('/process_registration', methods=['POST']): Decorates the process_registration function with a Flask route. This route is accessed through the URL '/process_registration' and only allows POST requests.
Request processing:
user_details = request.form: Retrieves the form data sent in the request body and assigns it to the user_details variable.
username = user_details.get('username'): Retrieves the 'username' field from the form data.
email = user_details.get('email'): Retrieves the 'email' field from the form data.
password = user_details.get('password'): Retrieves the 'password' field from the form data.
Querying the Neo4j graph database:
node1 = graph.nodes.match("Person", name=username, email=email).first(): Uses the NodeMatcher class from Py2neo to query the graph database. It attempts to find a node labeled as "Person" with matching 'name' and 'email' properties. The .first() method retrieves the first matching node.
Handling the query result and registration logic:
if node1:: Checks if a matching node was found in the graph database.
If true, it means the user already exists, and a message indicating the same is printed, and the response message is set accordingly.
If false, it means the user does not exist, and a new 'Person' node is created with the provided details (username, email, password) using the Node class. The graph.create(node) method is used to create the node in the graph database.
After the registration logic is performed, a response message is set accordingly.
Returning the response:
return jsonify(response), 200: Returns the response as a JSON object with an HTTP status code of 200 (indicating a successful request). The response contains a message indicating the outcome of the registration process.
The code assumes that the graph object represents a connected instance of the Neo4j graph database and the relevant Neo4j nodes are labeled as "Person" with 'name', 'email', and 'password' properties. Depending on the outcome of the registration process, the appropriate response message is returned as a JSON object.

 

This code snippet defines a Flask route for receiving user queries, processing them using AIML, and interacting with a Neo4j graph database. Here's an explanation of the code:
Route definition:
@app.route("/get"): Decorates the get_bot_response function with a Flask route. This route is accessed through the URL '/get'.
Getting user query:
query = request.args.get('msg'): Retrieves the 'msg' parameter from the query parameters of the request URL. This represents the user's query.
Preprocessing the query:
query = [spell(w) for w in query.split()]: Uses the autocorrect module to autocorrect any misspelled words in the query by splitting the query into words, autocorrecting each word, and rejoining them.
lemmas = [lemmatizer.lemmatize(word) for word in query]: Uses the WordNet lemmatizer to lemmatize each word in the query, reducing them to their base or root form.
question = " ".join(lemmas): Joins the lemmatized words back into a single string, representing the processed user query.
Generating a response using AIML:
response = k.respond(question): Passes the processed user query to the AIML kernel (k) using the respond method to generate a response.
Handling friendship relationship:
Checks if the processed query indicates a friendship relationship by checking if the word 'friend' is present in the lemmatized query.
If a friendship relationship is indicated, it extracts the names mentioned in the query (excluding the word 'friend').
It then searches for or creates the corresponding 'Person' nodes in the Neo4j graph database.
It creates a 'FRIEND' relationship between the user and friend nodes using the Relationship class, and saves it to the graph database.
Finally, it modifies the response message to indicate that the users are now friends.
Returning the response:
return str(response): Returns the response as a string. If a response is generated, it will be returned. Otherwise, a smiley face ":)" is returned.
Running the Flask application:
if __name__ == "__main__": app.run(port='8000'): Starts the Flask application and listens on port 8000 when the script is directly executed.
The code assumes that the AIML kernel (k) has been initialized and loaded with a brain file, as shown in the previous code snippet. Additionally, it assumes a connected Neo4j graph database represented by the graph object and labeled 'Person' nodes. The code processes user queries, generates responses using AIML, and handles the creation of friendship relationships in the Neo4j graph database when appropriate.
