> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n.md).

# Host n8n

You can self-host n8n on your own infrastructure, on-premises or in a private cloud, using Docker, Kubernetes, or npm.

All self-hosted installations use the same core product. Without a license key, n8n runs as the free Community edition. Adding a Business or Enterprise license key enables those editions. See [Compare editions](/deploy/host-n8n/community-edition-features.md) for the differences between the self-hosted editions.

## Choose your installation method <a href="#choose-your-installation-method" id="choose-your-installation-method"></a>

Select the installation method that best fits your technical requirements and infrastructure:

* **npm**

  **Best for:** Local development, testing, or simple single-server deployments.

  **Requirements:** Node.js installed on your system.

  Installs n8n directly using Node Package Manager. Quick to set up but requires managing Node.js versions and dependencies yourself.

  [npm installation guide](/deploy/host-n8n/install-options/install-with-npm.md)
* **Docker**

  **Best for:** Isolated environments, easy updates, and consistent deployments.

  **Requirements:** Docker installed on your system.

  Runs n8n in a container with all dependencies included. Simplifies installation and updates.

  [Docker installation guide](/deploy/host-n8n/install-options/install-with-docker.md)
* **AWS**

  Deploy on Amazon Web Services using EC2, ECS, or other AWS services.

  [AWS setup guide](/deploy/host-n8n/install-options/use-a-cloud-provider/deploy-to-aws.md)
* **Azure**

  Host n8n on Microsoft Azure with container instances or virtual machines.

  [Azure setup guide](/deploy/host-n8n/install-options/use-a-cloud-provider/deploy-to-azure.md)
* **Google Cloud**

  Run n8n on Google Cloud using Cloud Run or Kubernetes Engine.

  [Google Cloud Run](/deploy/host-n8n/install-options/use-a-cloud-provider/deploy-to-google-cloud-run.md) | [Kubernetes Engine](/deploy/host-n8n/install-options/use-a-cloud-provider/deploy-to-google-kubernetes.md)
* **DigitalOcean**

  Simple droplet-based hosting ideal for small to medium deployments.

  [DigitalOcean setup guide](/deploy/host-n8n/install-options/use-a-cloud-provider/deploy-to-digital-ocean.md)
* **Hetzner**

  Cost-effective European hosting option with excellent performance.

  [Hetzner setup guide](/deploy/host-n8n/install-options/use-a-cloud-provider/deploy-to-hetzner.md)
* **Heroku**

  Platform-as-a-service option for quick deployment with minimal configuration.

  [Heroku setup guide](/deploy/host-n8n/install-options/use-a-cloud-provider/deploy-to-heroku.md)
* **OpenShift**

  Enterprise Kubernetes platform for containerized applications.

  [OpenShift setup guide](/deploy/host-n8n/install-options/use-a-cloud-provider/deploy-to-openshift-local-crc.md)
* **Docker Compose**

  Multi-container setup ideal for production deployments with databases and additional services.

  [Docker Compose guide](/deploy/host-n8n/install-options/use-a-cloud-provider/use-docker-compose.md)
