---
layout: article
title: "RAG for company documents: when it actually helps"
description: "When RAG helps with company documents, when it adds unnecessary complexity, and what it costs to operate in production."
date: 2026-06-03
lang: en
topics: Applied AI · RAG
reading_time: 6 min read
language_current: English
language_label: Italiano
language_url: /a/2026/rag-company-documents/it/
---

Building an "AI" system that answers questions about company documents can look like a straightforward project: index the sources, connect a language model, and return an answer with a few references to the documents it used.

That works in a demo, but not once the system becomes part of daily work, because real documents are far more varied than people expect. There are different versions of the same file, documents that refer to other documents, attachments, permissions, legacy formats, zip archives, image-only PDFs, exceptions, unexpected questions, and users who still do not know how to phrase what they need.

The point is not just to make the model answer. It is to design everything that happens before the answer. In practice, you have to decide how to index documents, how to search them, how to use metadata and versions, how to rerank results, when to answer, and when to say that the sources are not enough. If those decisions stay implicit, the result becomes unpredictable.

## When RAG makes sense

RAG makes sense when the answer needs to be tied to a source. If a user asks what the rules are for travel reimbursement, a plausible answer is not enough. The system has to know which policy the information comes from, whether it is up to date, and whether it applies to that department. The same applies to contracts, procedures, and compliance documents, where an answer based on the wrong source can create problems.

RAG is also useful when the user's question does not use the same language as the documents. Someone may ask whether they can get a taxi reimbursed, while the policy talks about public transport expenses. Keyword search alone may not be enough. You may need embeddings, hybrid search, metadata, and often a reranker.

Another advantage of a well-designed system is that it can improve as it is used. If you record the questions users ask, feedback on the answers, and the mistakes the system makes, you can build a dataset that helps you understand where it fails.

Sometimes the problem is in the search pipeline: wrong or obsolete documents, incomplete metadata, chunking, retrieval, reranking, or the context passed to the model.

In other cases, you can use that dataset both for fine-tuning and to improve retrieval, prompts, and system evaluation. At a certain level, part of this process can also be automated: the system improves through user feedback instead of remaining stuck at the first version.

## When it becomes unnecessary complexity

There are cases where RAG adds more complexity than it solves.

If the document base is small, it may be enough to pass the documents directly to an LLM with a suitable context window. If the goal is to classify emails, route tickets, or extract a few fields from standard documents, a RAG system may be unnecessary complexity.

There is also the problem of the document base itself. If the company does not know which documents are valid, the first thing to do is work on cleanup, versions, ownership, and archiving. Otherwise, you risk building an elegant system on top of inconsistent content.

Even before the documents, there is a subtler problem: often the scope of the system has not been defined, and neither has the way its results will be measured. What answers should it provide? Based on which sources? When should it refuse to answer? And you do not evaluate it by asking three colleagues whether the answer "looks good": you need real questions, expected answers, and the right sources. A small set can be enough at the beginning, but it has to be explicit.

Sometimes the goal is not to answer questions about documents more effectively, but to trigger actions. If the system has to consult documents, call APIs, update states, open tickets, and apply business rules, then RAG is only one piece. You need to design a workflow, not just a document search system.

## The hidden costs of a RAG system

The cost of RAG is not only the token bill at the end of the month or the hardware you need if you run it in-house. It is the development of the system and everything that happens before and after the model call.

You have to extract text from PDFs, DOCX files, HTML pages, scans, and attachments. Sometimes you also have to call APIs, query databases, or handle another long list of integrations. You have to decide how to split documents, which metadata to keep, and how to handle versions. Then you have to index, search, filter, rerank, assemble context, generate the answer, cite sources, and record what happened when something goes wrong.

Development is a significant part of the project: integrations with existing archives, handling parsing errors, dedicated techniques to identify and extract metadata and structured data, updating indexes when documents change, evaluating answers, and tools to understand why the system answered in a certain way.

Then there are the parts that stay out of the demo: incremental updates, ingestion and per-question costs, source tracing, monitoring, and regressions when you change the model or chunking strategy.

This does not mean RAG should be avoided. It only means it should be treated as a production system, not as a shortcut to make a model talk to documents.

## How to decide

The first question is whether what you are looking for needs to be traceable to an up-to-date source. If the answer is no, RAG is probably not the right starting point. If the answer is yes, the next question is whether the document base is organized enough to build a RAG system on top of it. If it is not, the first job is to fix the documents: a messy document base produces wrong answers with no easy way to notice.

The best choice is almost always to define the system first: what it needs to know, how it should behave, what data it receives, how it integrates with the process, and how it fits into document management.
Only when the scope is clear does it make sense to start working on RAG.

In the next article I will go into the technical details: parsing, chunking, metadata, hybrid search, reranking, and generation.
