---
title: Learning RAG
layout: series
series: learning-rag
permalink: /series/learning-rag/
date: 2026-09-10
description: Learning retrieval-augmented generation from the basic idea through retrieval, evaluation, and real implementations.
tags: [rag, llm, ai]
parts:
  - number: I
    title: Foundations
    posts:
      - order: 1
        title: Why RAG?
      - order: 2
        title: "LLM Knowledge: Parameters, Context and External Information"
      - order: 3
        title: Retrieval-Augmented Generation
      - order: 4
        title: Documents, Tokens and Chunking
  - number: II
    title: Retrieval From Scratch
    posts:
      - order: 5
        title: Embeddings and Vector Spaces
      - order: 6
        title: Similarity Search From Scratch
      - order: 7
        title: Information Retrieval and BM25
      - order: 8
        title: Approximate Nearest Neighbors
  - number: III
    title: Building RAG
    posts:
      - order: 9
        title: Our First Complete RAG
      - order: 10
        title: Hybrid Search and Reranking
      - order: 11
        title: Context Construction and Grounding
  - number: IV
    title: Measuring RAG
    posts:
      - order: 12
        title: Evaluating Retrieval
      - order: 13
        title: Evaluating Answers
      - order: 14
        title: Debugging RAG
  - number: V
    title: Production RAG
    posts:
      - order: 15
        title: Ingestion and Document Lifecycle
      - order: 16
        title: Architecture and APIs
      - order: 17
        title: Observability, Latency and Cost
      - order: 18
        title: Security
      - order: 19
        title: Deployment and Operations
  - number: VI
    title: Beyond Basic RAG
    posts:
      - order: 20
        title: Advanced Retrieval
      - order: 21
        title: Local vs Hosted Models
      - order: 22
        title: What I Learned Building Production RAG
---

## Goal

Retrieval-augmented generation, usually shortened to **RAG**, sounds simple at first: find useful information and give it to a language model before asking for an answer. Of course, once you start digging into it, every part opens another box: documents, chunks, embeddings, retrieval, reranking, prompts, evaluation...
RAG (Retrrieval augmented generation) is a way to use a language model to get specific (and useful) informatin before askin for an answer, it's a topic I have not learnt about and there are two things that are motivating me to learn it:

- I like learning (and AI)

- Have seen more and more mentioned in data engineering jd 


## What we're building

The idea is to start learning the basic concepts and then first work with, the smallest RAG pipeline that actually works and gradually turn it into something closer to a production system. That means building retrieval ourselves before reaching for abstractions, measuring whether it works, and then dealing with the less glamorous parts such as document updates, latency, cost, security, and deployment.

## Architecture

Really the system will have mostly two parts (at the time of this writting):

- An **ingestion path** that loads documents, splits them into chunks, creates searchable representations, and keeps the index updated.
- A **query path** that retrieves relevant information, improves the results when needed, constructs the context, and asks a model to produce a grounded answer.
