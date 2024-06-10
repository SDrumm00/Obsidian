---
Created:
  - 20240320 9:27 AM
aliases: 
tags:
  - Generative-AI
  - AI
  - Machine-Learning
  - home-automation
  - SafeHaven
  - Building-A-Second-Brain
Subject: Project SafeHaven GenAI at Home
---
-----------------
# Project SafeHaven Abstract

## Problem Statement
Having a self-hosted assistant that is powered by a large language model and RAG Architecture is helpful for many reasons. Myself personally as a [[A Second Brain]] and also for a quicker and more reliable means than Google to search for appropriately accurate answers.
## Use Cases
My current uses cases are:
1) [[A Second Brain]]
2) Training certifications that I have listed [[2024 Learning Goals]]
3) Security
4) Understanding and ensuring that information is accurate more than the ones offered on the internet
## Requirements
1) Must be on my own infrastructure and not a web service.
2) Should use my homelab virtualization server over my local workstation
	1) Looking into [[ProxMox]] (again)
	2) Already have a [[TrueNAS]] deployment which is fine
## Deployment Strategy
1) User a pre-built solution
	1) [Bionic-GPT](https://bionic-gpt.com/)
	2) [GPT4ALL](https://gpt4all.io/index.html)
2) Build my own from the ground up
	-  Fine grained control over each moving part (but is it worth it right now or over time?
		1) [Knime](https://www.knime.com/) has some LLM CI/CD Pipeline toolkits
		2) [Weaviate](https://weaviate.io/) looks like a promising vector database
		3) [ChromaDB](https://docs.trychroma.com/) is another good looking option
		4) Milvus as well for vectorDB's

## Take Aways
Do some now in order to have something. Then, begin to re-deploy a more fine tuned solution.
Great way to walk through how to replace moving parts and practice DevOps, DataOps and MLOps all in one place.

## Decisions
I chose to use [GPT4ALL](https://gpt4all.io/index.html) for my first local deployment. It is easy enough to use and I can see how it was done by others. That helps me see the outcome of my work when I do it manually in the next iteration.

I'll add more details about [[GPT4ALL]] in time.

