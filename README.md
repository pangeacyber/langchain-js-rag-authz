# User-based Access Control with RAG for LangChain in JavaScript

A sample JavaScript app that demonstrates integrating Pangea's [AuthZ][] service
into a LangChain app to apply user-based authorization to control access to
files for a RAG workflow.

## Prerequisites

- Node.js v22.
- A [Pangea account][Pangea signup] with AuthZ enabled.
- An [OpenAI API key][OpenAI API keys].

## Setup

### Pangea AuthZ

The setup in AuthZ should look something like this:

#### Resource types

| Name        | Permissions |
| ----------- | ----------- |
| engineering | read        |
| finance     | read        |

#### Roles & access

> [!TIP]
> At this point you need to create 2 new Roles under the `Roles & Access` tab in
> the Pangea console named `engineering` and `finance`.

##### Role: engineering

| Resource type | Permissions (read) |
| ------------- | ------------------ |
| engineering   | ✔️                 |
| finance       | ❌                 |

##### Role: finance

| Resource type | Permissions (read) |
| ------------- | ------------------ |
| engineering   | ❌                 |
| finance       | ✔️                 |

#### Assigned roles & relations

| Subject type | Subject ID | Role/Relation |
| ------------ | ---------- | ------------- |
| user         | alice      | engineering   |
| user         | bob        | finance       |

### Repository

```shell
git clone https://github.com/pangeacyber/langchain-js-rag-authz.git
cd langchain-js-rag-authz
npm install
cp .env.example .env
```

Fill in the values in `.env` and then the app can be run like so:

Assuming user "alice" has permission to see engineering documents, they can
query the LLM on information regarding those documents:

```
$ npm run demo -- --user alice "What is the software architecture of the company?"

The company's software architecture includes a frontend built with React.js and
Material-UI, while the backend utilizes Node.js and Express.js. MongoDB is used
for the database, with JSON Web Tokens (JWT) and OAuth 2.0 for authentication
and authorization. Version control is managed through Git and GitHub.
```

But they cannot query finance information:

```
$ npm run demo -- --user alice "What is the top salary in the Engineering department?"

I don't know.
```

And vice versa for "bob", who is in finance but not engineering.

[AuthZ]: https://pangea.cloud/docs/authz/
[Pangea signup]: https://pangea.cloud/signup
[OpenAI API keys]: https://platform.openai.com/api-keys
