cf-medusa
=========

Proof of concept for hosting a MedusaJS store + a Next.js storefront using a Docker cluster, with Cloudflare Tunnels for ingress.

## Prerequisites:

1. The domain needs to be hosted on Cloudflare (ie, use Cloudflare's name servers) on at least a Free plan.
2. You'll need a Cloudflare Tunnel Token - create one via the Zero Trust dashboard (Networks > Tunnels > Create, Cloudflared, set the domain name).
3. The cluster needs to run on a machine that has Docker installed, ideally Ubuntu Linux. This machine need not be a VM, it can literally be a tower box plugged into a wall at your house, provided it has somewhat decent internet access.

The overall solution: The store runs in the cluster on your box, and the ingress container "punches out" to the internet by connecting to Cloudflare's edge, and Cloudflare then proxies all incoming traffic on the domain, to your setup.

## Docker Cluster

The cluster consists of a handful of containers:

* `store` - Container running the NextJS storefront
* `backend` - Container running the MedusaJS backend
* `db` - PostgreSQL container for storage
* `tunnel` - Cloudflare container for ingress

The `db` and `tunnel` containers will pull from the public Docker registry, but we'll need to build custom containers for the `store` and `backend` to contain all the code we're hosting.

