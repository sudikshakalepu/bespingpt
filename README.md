# bespingpt

The application's seamless integration of the RAG (Retrieval Augmented Generation) pattern enhances the customer support experience for Bespin Global MEA, serving as a real-life architecture reference and application sample for JavaScript developers using Azure. This BespinGPT use-case demonstrates the power of Azure OpenAI Service and Azure AI Search SDK, highlighting how companies of any size can provide their customers with a personalized and comprehensive support system.
 App Architecture:
All components of this application, including the static frontend and backend services, are deployed as a unified architecture unit to their respective managed infrastructure services. We chose Azure Static Web Apps for the web application and Azure Container Apps for the backend services.
During initial deployment or subsequent workflow executions, the Azure AI Search indexer service ingests and indexes data to make the application operational. When an end-user
submits a query via the frontend web component to the assistant, the request, along with a set of headers, is directed to a backend Azure OpenAI service. This service processes the request according to configured options, such as the context approach (e.g., "retrieve then read"), the retrieval mode (full-text, vector, or hybrid), and semantic features like "semantic ranker" for optimized response relevance or "semantic captions." Additionally, it supports streamed responses using HTTP Readable Streams to generate responses that the frontend can render.
UX Settings Customization:
This application sample is tailored for JavaScript and TypeScript developers, facilitating easy integration with existing applications built using modern frameworks. It includes a web component that can be embedded into any frontend and integrates smoothly with any backend that adheres to the Chat Protocol specification. While the original sample is implemented end-to-end with JavaScript/TypeScript, it can also be connected to a Python backend.
Some considerations to keep in mind:
OpenAI Capacity: The default token processing rate is 30K tokens per minute, which roughly translates to about 30 conversations per minute, assuming each message/response pair is 1K tokens. You can increase this capacity by modifying the chatGptDeploymentCapacity and embeddingDeploymentCapacity parameters in infra/main.bicep to reach your account's maximum capacity. Additionally, you can check the Quotas tab in Azure OpenAI Studio to understand your available capacity.
Azure Storage: The default storage account uses the Standard_LRS SKU. For improved resiliency, it's recommended to use Standard_ZRS for production deployments, which can be specified using the sku property under the storage module in infra/main.bicep.
Azure AI Search: The default search service uses the Standard SKU with the free semantic search option, providing 1000 free queries per month. If your application will handle more than 1000 queries, consider changing semanticSearch to "standard" or disabling semantic search in the request options. If you encounter capacity errors, you can increase the number of replicas by adjusting replicaCount in infra/core/search/search- services.bicep or by manually scaling through the Azure Portal.
Azure Container Apps: The default container app setup includes 1 vCPU core and 2 GB RAM per container, with autoscaling enabled. The minimum number of replicas is set to 1 and the maximum to 10. You can adjust the vCPU and RAM capacity in the template and define
 
your own autoscaling rules based on load. For further details, refer to the "Set scaling rules in Azure Container Apps" documentation.
Authentication: By default, the deployed app is publicly accessible. It is recommended to restrict access to authenticated users. Refer to the "Enabling authentication" section for instructions on how to enable authentication.
Networking: It's recommended to deploy inside a Virtual Network. For internal enterprise use, consider using a private DNS zone. Additionally, using Azure API Management (APIM) can provide firewalls and other protections. For more details, see the "Azure OpenAI Landing Zone reference architecture" documentation.
