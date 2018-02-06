# aws-api


### Java Code
```
package lucas.ko;

import java.util.List;

import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.AWSCredentialsProvider;
import com.amazonaws.auth.AWSStaticCredentialsProvider;
import com.amazonaws.auth.BasicAWSCredentials;
import com.amazonaws.auth.DefaultAWSCredentialsProviderChain;
import com.amazonaws.services.cloudtrail.processinglibrary.AWSCloudTrailProcessingExecutor;
import com.amazonaws.services.cloudtrail.processinglibrary.configuration.ClientConfiguration;
import com.amazonaws.services.cloudtrail.processinglibrary.configuration.ProcessingConfiguration;
import com.amazonaws.services.cloudtrail.processinglibrary.exceptions.CallbackException;
import com.amazonaws.services.cloudtrail.processinglibrary.interfaces.EventsProcessor;
import com.amazonaws.services.cloudtrail.processinglibrary.model.CloudTrailEvent;
import com.amazonaws.services.ec2.AmazonEC2;
import com.amazonaws.services.ec2.AmazonEC2Client;

public class awsParser {

	public static void main(String[] args) throws InterruptedException {
		// TODO Auto-generated method stub

		String access_key_id = "";
		String secret_key_id = "";

		AWSCredentials awsCreds = new BasicAWSCredentials(access_key_id, secret_key_id);

		ClientConfiguration basicConfig = new ClientConfiguration(
				"https://sqs.ap-northeast-1.amazonaws.com/xxxxx/sqsname", new AWSStaticCredentialsProvider(awsCreds));
	
		
		basicConfig.setEnableRawEventInfo(true);
		basicConfig.setThreadCount(1);
		basicConfig.setMaxEventsPerEmit(10);
		basicConfig.setSqsRegion("ap-northeast-1");
		basicConfig.visibilityTimeout = 60;
		basicConfig.setThreadTerminationDelaySeconds(60);
		basicConfig.setS3Region("ap-northeast-1");
	
		
		
		EventsProcessor eventsProcessor = new EventsProcessor() {

			public void process(List<CloudTrailEvent> events) throws CallbackException {
				// TODO Auto-generated method stub
				int i = 0;
				System.out.println(">> process");
				for (CloudTrailEvent event : events) {
					System.out.println(String.format("Process event %d : %s", i++, event.getEventData()));
				}
			}
		};

		AWSCloudTrailProcessingExecutor executor = new AWSCloudTrailProcessingExecutor.Builder(
				eventsProcessor, basicConfig).build();
		
		System.out.println("start");
		executor.start();

		Thread.sleep(24 * 60 * 60 * 1000); // run for a while (optional)
		System.out.println("stop");
		executor.stop(); // optional

	}

}

```


###pom.xml
```
		<dependency>
			<groupId>com.amazonaws</groupId>
			<artifactId>aws-java-sdk</artifactId>
			<version>1.11.267</version>
		</dependency>


		<dependency>
			<groupId>com.amazonaws</groupId>
			<artifactId>aws-cloudtrail-processing-library</artifactId>
			<version>1.1.1</version>
		</dependency>

```
