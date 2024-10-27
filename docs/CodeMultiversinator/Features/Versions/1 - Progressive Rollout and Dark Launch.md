
## Progressive Rollout and Dark Launch

The first version will aim the ability to use the dark launch and progressive rollout with the platform. With this feature the deliver of critical tasks will be made safely. To ensure the developers will have the right tools to do it, some components will be delivered:

### Library

To simplify interactions with the experiment platform, a dedicated library will be developed for use on the service side. This library will handle requests to determine the customer’s experiment group, hiding the underlying complexity from developers. By abstracting these details, developers can seamlessly access experiment group information without needing an in-depth understanding of the experiment platform's internal workings.

By encapsulating the intricacies of the experiment platform within this library, we also ensure that core details remain contained within the platform’s domain, reducing potential for domain leakage and enhancing maintainability. Ownership of this library will reside with the experiment platform team, who will oversee its development, updates, and integration with evolving platform needs. This setup preserves platform boundaries, optimizes developer experience, and promotes consistency across services.

### Decider Service

To effectively manage experiment group assignments, a platform service will be developed to handle requests from product services. This platform service will determine a customer’s group for a specific experiment, optimizing response times and ensuring reliable access to experiment data.

The service will use two databases: Redis for caching experiment data and a persistent database as a backup. When a product service requests the customer’s experiment group, the platform service will first check Redis for the relevant experiment data. Redis serves as the primary source for experiment data, allowing for fast retrieval and low-latency responses. In the event of an issue with Redis, the service will fall back to the persistent database, ensuring continuity and reliability in data access.

This setup enables high performance, scalability, and resilience, accommodating large request volumes with minimal latency while maintaining a backup for system stability. By centralizing group data management in this way, the platform service provides efficient, reliable customer segmentation to enhance user experience across services.

### Controller Service

An additional service will be introduced to act as a Backend for Frontend (BFF) specifically for managing experiments. This BFF service will provide endpoints for creating, editing, and deleting experiment data, ensuring synchronized updates to both Redis (for fast access) and the persistent database (for backup and historical consistency).

In its current version, the interface for this BFF service will be accessible through cURL operations, providing a straightforward way to perform these experiment management tasks. This approach allows easy command-line interaction for development and testing while maintaining direct control over experiment data and its propagation across storage layers.

The BFF’s role is to serve as a dedicated access point for experiment management, streamlining operations and maintaining the integrity and availability of experiment data throughout the system.

### Real time monitoring

To ensure effective monitoring of dark launches and progressive rollouts, we’ll use **Prometheus** and **Grafana**. Together, these tools provide a comprehensive monitoring and visualization solution that enables real-time insights into our system’s performance.

#### Prometheus: Real-Time Metric Collection

Prometheus is a robust open-source monitoring system designed for real-time metric collection and alerting. By integrating Prometheus, we can gather detailed metrics across various parts of our system, allowing us to capture critical performance data during feature rollouts.

#### Grafana: Real-Time Visualization and Dashboards

Grafana complements Prometheus by offering a powerful visualization layer for our metrics. It lets us create intuitive, real-time dashboards to monitor rollout health and performance at a glance, ensuring that data insights are easily accessible.

### Architecture Overview

<img src="https://github.com/user-attachments/assets/6d29995d-e941-477e-a700-ce4f26862174">

