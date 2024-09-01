import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import com.fasterxml.jackson.databind.ObjectMapper;

		// Convert Java object to JSON string using Jackson ObjectMapper
        ObjectMapper objectMapper = new ObjectMapper();
		String jsonRequestBody = objectMapper.writeValueAsString(requestBody);
		
		 // Create HttpClient
        HttpClient client = HttpClient.newHttpClient();

        // Create HttpRequest
        HttpRequest request = HttpRequest.newBuilder()
            .uri(new URI("http://example.com/api"))
            .header("Content-Type", "application/json")
            .header("Custom-Header-1", "HeaderValue1")
            .header("Custom-Header-2", "HeaderValue2")
            .POST(HttpRequest.BodyPublishers.ofString(jsonRequestBody))
            .build();
			
			
		 // Send request and get raw response
        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString()); //HttpResponse.BodyHandlers.ofByteArray()
		
		// Send request and get raw response as byte array
        HttpResponse<byte[]> response = client.send(request, HttpResponse.BodyHandlers.ofByteArray());

		
		<dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.15.2</version> <!-- Use the latest version if available -->
    </dependency>
	
	
	
***************************************************************************************************************
<dependency>
    <groupId>org.apache.httpcomponents.client5</groupId>
    <artifactId>httpclient5</artifactId>
    <version>5.2</version> <!-- Ensure the latest version -->
</dependency>



import org.apache.hc.client5.http.classic.methods.HttpPost;
import org.apache.hc.client5.http.impl.classic.CloseableHttpClient;
import org.apache.hc.client5.http.impl.classic.CloseableHttpResponse;
import org.apache.hc.client5.http.impl.classic.HttpClients;
import org.apache.hc.core5.http.io.entity.StringEntity;

public class ApacheHttpClientExample {

    public static void main(String[] args) throws Exception {
        try (CloseableHttpClient client = HttpClients.createDefault()) {
            HttpPost post = new HttpPost("http://example.com/api");
            post.addHeader("Content-Type", "application/json");
            post.addHeader("Custom-Header-1", "HeaderValue1");

            // Convert your Java object to JSON
            ObjectMapper objectMapper = new ObjectMapper();
            String jsonRequestBody = objectMapper.writeValueAsString(new MyRequestBody());

            post.setEntity(new StringEntity(jsonRequestBody));

            try (CloseableHttpResponse response = client.execute(post)) {
                // Get raw response as a byte array
                byte[] rawResponse = response.getEntity().getContent().readAllBytes();
                System.out.println("Response length: " + rawResponse.length);
            }
        }
    }
}

*******************************************************************************************************

<dependency>
    <groupId>com.squareup.okhttp3</groupId>
    <artifactId>okhttp</artifactId>
    <version>4.11.0</version>
</dependency>


import okhttp3.*;

public class OkHttpClientExample {

    public static void main(String[] args) throws Exception {
        OkHttpClient client = new OkHttpClient();

        // Convert your Java object to JSON
        ObjectMapper objectMapper = new ObjectMapper();
        String jsonRequestBody = objectMapper.writeValueAsString(new MyRequestBody());

        RequestBody body = RequestBody.create(jsonRequestBody, MediaType.get("application/json; charset=utf-8"));
        Request request = new Request.Builder()
            .url("http://example.com/api")
            .header("Custom-Header-1", "HeaderValue1")
            .post(body)
            .build();

        try (Response response = client.newCall(request).execute()) {
            byte[] rawResponse = response.body().bytes();
            System.out.println("Response length: " + rawResponse.length);
        }
    }
}
