# CENG319-Assignment-3

# Volley
Introduction:
Volley is a HTTP library that was developed by Google. The volley library is used to transfer data over the internet easier and faster. This library is available in the Android Open Source Project repository. 
With volley, you get many features and benefits, the volley library allows for automatic scheduling of network requests, volley allows for many different network connections to be working at once, it contains support for request prioritizations, volley allows you to cancel and/or block a request, it allows for transparent disk and memory response caching standard HTTP cache coherence(refers to a number of ways to make sure that all of the caches of the resource have the same data, and that all of the data in the caches make sense), allows for easier management of the user interface with data that was fetched at the same time through the network, allows for easier customization, and contains different debugging and tracing tools.
Volley caches the data. When a user requests the same data volley will directly show it from the cache saving resource instead of calling from the server, this makes the user experience better. Without volley, when the content would be fetched in the portrait mode and then the screen changes to landscape mode, the activity would be destroyed during the swap and this would cause the content to have to be fetched again. As it will always be the same data being fetched, it is a waste of resources and does not make for a good experience.

History:
As stated earlier, volley was developed by Google and was introduced during Google I/O in 2013.  Google created volley as there was an absence of a networking class that would be capable of working without messing with or bothering the user experience, in the Android SDK. 
Before volley was created, the Java class and the Apache were the only tools that programmers could use to develop a system that includes REST (this is a simple way to organize interactions between different independent systems, allows you to interact with minimal overhead with clients as diverse as mobile phones and many other different websites.) between the client and a remote backend. These two classes had many problems with them, one being they weren’t exempt from bugs, volley was created to be a better version of the two combined classes.

The Major Methods/ Attributes:
Request – This takes the parameters as the method type, the URL of the resource, and event listeners. After this, depending on the type of request, you may be prompted for more variables.
RequestQueue – This manages worker threads and delivers the parsed results back to the main thread.
Volley.newRequest – This sets up a RequestQueue object, using the default values that were defined by volley
Volley.newRequestQueue – This sets up a RequestQueue object, using the default values that were defined by volley, and also starts the queue.
Request.Method.GET – The GET is used to read.
Request.Method.POST – The POST is used to create.
Request.Method.PUT – The PUT is used to update and/or replace.
Request.Method.DELETE – The DELETE is used to delete.
Request.Method.PATCH – The PATCH is used to update and/or modify.

Example Project:
Below, I have attached an example project of how the volley library is being used and appears to all of the users. All of the code provided is necessary for the code to run properly and there are also comments included to allow for easier understanding of the code.
For volley to be used in android studio you must also change the build.gradle and the manifest.xml a little bit. In the build.gradle, the line compile ‘com.android.volley:volley:1.0.0’ must be added. This line adds the dependency files for volley. In the manifest.xml, the line <uses-permission android:name=”android.permission.INTERNET” /> must also be added. This line allows for the use of internet and this is needed as we will be collecting the data from the internet.

Code:
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
 
import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.JsonObjectRequest;
import com.android.volley.toolbox.Volley;
 
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;
 
public class MainActivity extends AppCompatActivity {
   
    private TextView mTextViewResult;
    private RequestQueue mQueue;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mTextViewResult = findViewById(R.id.text_view_result);
        Button buttonTest = findViewById(R.id.test);
 
//Creating an instance of the RequestQueue
        mQueue = Volley.newRequestQueue(this);

        buttonTest.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                jsonParse(); //Starting the parse method
            }
        });
    }
 
    private void jsonParse() {
 
        String url = "https://api.myjson.com/bins/oeg6m"; // This is my URL that contains the JSON code.
 
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, url, null,
                new Response.Listener<JSONObject>() {          //will be called when there will be a response back
                    @Override
                    public void onResponse(JSONObject response) {
                        try {
                            JSONArray jsonArray = response.getJSONArray("people"); //This is getting the JSON array
 
                            for (int i = 0; i < jsonArray.length(); i++) {    //This is getting each object in the array
                                JSONObject people = jsonArray.getJSONObject(i);
 
//These are the variables that will contain the values
                                String firstName = people.getString("firstname");
                                int age = people.getInt("age");
                                String mail = people.getString("mail");
 
//Display the results in a TextView
                                mTextViewResult.append(firstName + ", " + String.valueOf(age) + ", " + mail + "\n\n"); 
                            }
                        } catch (JSONException e) {
                            e.printStackTrace();
                        }
                    }
                }, new Response.ErrorListener() {      //will be called if there was an error
            @Override
            public void onErrorResponse(VolleyError error) {
                error.printStackTrace();
            }
        });
 
        mQueue.add(request);  //adds the request to the queue
    }
}

References:
https://developer.android.com/training/volley/ 
https://developer.android.com/training/volley/simple 
https://abhiandroid.com/programming/volley#Volley_Basic_HTTP_Example_In_Android_Studio 
https://code.tutsplus.com/tutorials/an-introduction-to-volley--cms-23800 
