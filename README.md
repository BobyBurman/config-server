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
