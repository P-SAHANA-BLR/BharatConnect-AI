# BharatConnect AI

An inclusive, AI-powered conversational application designed to help rural and semi-urban youth, elderly citizens, and low digitally literate users in India discover and understand government schemes, skill programs, and public opportunities.

## Project Overview

BharatConnect AI addresses the critical problem of accessibility barriers—including language constraints and complex portals—that prevent millions of Indians from accessing beneficial government and private schemes.

## Features

- **Language-First UX**: Mandatory language selection before any interaction
- **Profile-Once Pattern**: Collect user age and education once, reuse automatically
- **Conversational Interface**: Accept keywords or incomplete sentences
- **Personalized Discovery**: Filter schemes based on user profile
- **Grounded AI**: No hallucinations, all responses based on real data
- **Hybrid RAG Pipeline**: Retrieve → Fetch → Cache → Respond

## Tech Stack

- **Backend**: Python 3.10+ with FastAPI
- **Database**: Supabase (PostgreSQL) with pgvector
- **Embeddings**: Sentence-Transformers
- **LLM**: Google Gemini (swappable with AWS Bedrock)
- **Frontend**: React/Vue.js (mobile-first)

## Documentation

- [Requirements](/.kiro/specs/bharatconnect-ai/requirements.md) - Complete requirements with acceptance criteria
- [Design](/.kiro/specs/bharatconnect-ai/design.md) - Architecture and component design
- [Tasks](/.kiro/specs/bharatconnect-ai/tasks.md) - Implementation plan with 80+ tasks

## Getting Started

See the [tasks.md](/.kiro/specs/bharatconnect-ai/tasks.md) file for the complete implementation plan.


