
.. code-block:: bash

    curl -s --user 'api:YOUR_API_KEY' \
	https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/unsubscribes \
	-F address='bob@example.com' \
	-F tag='*'

.. code-block:: java

 import javax.ws.rs.client.Client;
 import javax.ws.rs.client.ClientBuilder;
 import javax.ws.rs.client.Entity;
 import javax.ws.rs.client.WebTarget;

 import javax.ws.rs.core.Form;
 import javax.ws.rs.core.MediaType;

 import org.glassfish.jersey.client.authentication.HttpAuthenticationFeature;

 public class MGSample {

     // ...

     public static ClientResponse AddUnsubscribeAll() {

         Client client = ClientBuilder.newClient();
         client.register(HttpAuthenticationFeature.basic(
             "api",
             "YOUR_API_KEY"
         ));

         WebTarget mgRoot = client.target("https://api.mailgun.net/v3");

         Form reqData = new Form();
         reqData.param("address", "bob@example.com");
         reqData.param("tag", "*");

         return mgRoot
             .path("/{domain}/unsubscribes")
             .resolveTemplate("domain", "YOUR_DOMAIN_NAME")
             .request(MediaType.APPLICATION_FORM_URLENCODED)
             .buildPost(Entity.entity(reqData, MediaType.APPLICATION_FORM_URLENCODED))
             .invoke(ClientResponse.class);
     }
 }

.. code-block:: php

  # Include the Autoloader (see "Libraries" for install instructions)
  require 'vendor/autoload.php';
  use Mailgun\Mailgun;

  # Instantiate the client.
  $mgClient = new Mailgun('YOUR_API_KEY');
  $domain = 'YOUR_DOMAIN_NAME';

  # Issue the call to the client.
  $result = $mgClient->post("$domain/unsubscribes", array(
      'address' => 'bob@example.com',
      'tag'     => '*'
  ));

.. code-block:: py

 def unsubscribe_from_all():
     return requests.post(
         "https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/unsubscribes",
         auth=("api", "YOUR_API_KEY"),
         data={'address':'bob@example.com', 'tag': '*'})

.. code-block:: rb

 def unsubscribe_from_all
   RestClient.post "https://api:YOUR_API_KEY"\
   "@api.mailgun.net/v3/YOUR_DOMAIN_NAME/unsubscribes",
   :address => 'bob@example.com',
   :tag => '*'
 end

.. code-block:: csharp

 using System;
 using System.IO;
 using RestSharp;
 using RestSharp.Authenticators;
 
 public class AddUnsubscribeAllChunk
 {
 
     public static void Main (string[] args)
     {
         Console.WriteLine (UnsubscribeFromAll ().Content.ToString ());
     }
 
     public static IRestResponse UnsubscribeFromAll ()
     {
         RestClient client = new RestClient ();
         client.BaseUrl = new Uri ("https://api.mailgun.net/v3");
         client.Authenticator =
             new HttpBasicAuthenticator ("api",
                                         "YOUR_API_KEY");
         RestRequest request = new RestRequest ();
         request.Resource = "{domain}/unsubscribes";
         request.AddParameter ("domain", "YOUR_DOMAIN_NAME", ParameterType.UrlSegment);
         request.AddParameter ("address", "bob@example.com");
         request.AddParameter ("tag", "*");
         request.Method = Method.POST;
         return client.Execute (request);
     }
 
 }

.. code-block:: go

 func CreateUnsubscription(domain, apiKey string) {
   mg := mailgun.NewMailgun(domain, apiKey, "")
   return mg.Unsubscribe("bob@example.com", "*")
 }
