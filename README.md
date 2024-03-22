# Nuts Discovery Server example
This repository portrays how to deploy a Nuts Discovery Server for a fictional "debate club" use case,
in which alumni who've gotten a degree from a university can (publicly) register themselves and their degree for participation in debates.

In a real-world use case, the person holding the degree credential would have it in their own (self-sovereign) wallet,
but in this case it is "managed" and thus registered by the university.

## Usage

Start the Docker Compose file. Then either run the .http file using IntelliJ or using the following Docker command:

```shell
docker run --rm --network=nuts-discovery-server-example_default -i -t -v './issue-DIDs-and-VCs-docker.http:/workdir/requests.http' jetbrains/intellij-http-client -L VERBOSE requests.http
```

This HTTP request script performs the following actions:

1. Create DID for Awesome College
2. Create DID for the student
3. Issue a degree credential for the student
4. Load the degree credential into the student's wallet
5. Activate the debate club Discovery Service for the student's wallet. This causes a Verifiable Presentation to be registered on the Discovery Service.
6. Search for registrations on Discovery Service where `credentialSubject.degree.type` is `BachelorDegree` 

## Components

The Docker Compose setup contains the following components:

### `discoveryserver`
Nuts Node configured as Discovery Server, serving the alumni use case.

### `discoveryserver_db`
Postgres database for the Discovery Server.

### `awesome_college_node`
Nuts Node configured as a "university" node, which can register alumni and their degrees.

### `awesome_college`
NGINX proxy for `awesome_college` node, required for HTTPS.

## Notes

Notes on the Discovery Definition found in `config/debate_club.json`:

- It limits the allowed issuers of the `UniversityDegreeCredential` to 2 issuers (one in this setup, and another, fictional one)
- It specifies an `id` for the `credentialSubject.degree.type` field, which causes it to be returned in searches in the `fields` response field.