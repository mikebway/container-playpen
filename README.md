# Container Playpen

Thie repo contains no code or anything else of much use to anybody other than myself. It is only public
in order that I can more easily play with a toy [MicroK8s](https://microk8s.io/) installation without
struggling with private container repository keys.

None of the Docker images stored here have any practical use or value, intellectual or otherwise. They
are just marginally more sophisticated than "Hello, world!" though they could be used to demonstrate intra-pod
and extra-pod communication between containers.

## Kubernetes Unable to Resolve Digest for GitHub Image Package

At the time of writing (December 8, 2019), [GitHub Packages](https://github.com/features/packages) still appears
to have some teething problems as a Docker container image repository when referenced by a Kubernetes pod deployment.
It's unclear whether this is an issue with GitHub's implementation of the Docker repository protocol, with the
MicroK8s version I am running (v1.16.3 - latest stable), or with Kubernetes in general but every attempt to
deploy the pod described by [`partner.yaml`](partner.yaml) fails with this message:

```text
Failed to pull image "docker.pkg.github.com/mikebway/container-playpen/partner-grpc:1.0.0": rpc error: \
code = Unknown desc = failed to resolve image \
"docker.pkg.github.com/mikebway/container-playpen/partner-grpc:1.0.0": no available registry endpoint: \
could not resolve digest for docker.pkg.github.com/mikebway/container-playpen/partner-grpc:1.0.0
```

The important part of that being that Kubernetes `could not resolve digest` for the image, i.e. could not validate
that the image's integrity had not been compromised. This point was only reached after typing the package name
correctly (fat fingers), and configuring the `partner.yaml` to reference a Kubernetes secret with an appropriate
GitHub personal access token. Those issues gave difference errors. `could not resolve digest` has so far proved
more intractible.
