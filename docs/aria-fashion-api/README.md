# Aria Fashion API Reference

Live documentation: [View API Reference](https://shruti-rajpurohit.github.io/Technical-writing-portfolio/docs/aria-fashion-api/index.html)

## Overview

Welcome to the complete API documentation for the Aria Fashion API, the backend for the full-stack AI Fashion Stylist web application I developed and deployed from scratch: [Aria Fashion Stylist](https://aria-fashion-stylist.onrender.com).

The API handles user authentication, style profile management, AI-powered chat, and computer vision-based image analysis. I wrote this reference by reading the codebase directly and documenting every endpoint, request schema, response schema, and error code.

## What's documented

- Authentication endpoints (signup, login)
- Style profile creation and retrieval
- AI chat endpoint with conversation history and chat management
- Chat endpoints: POST /api/chats, GET /api/chats, GET /api/chats/{chatId}, and DELETE /api/chats/{chatId}
- Chat full behavior using replyCount >= 8 and nudge behavior using replyCount >= 5
- Updated POST /api/chat request/response fields: chatId, message, imageAnalysis, response, nudge, full, error, and CHAT_FULL
- Vision endpoints for profile photo and reference image analysis, including updated /api/analyze-chat-images
- Multiple reference images per chat message, up to 3 images, with MAX_CHAT_IMAGE_SIZE = 10 * 1024 * 1024, the images FormData field, and image resizing
- Complete request/response schemas with examples
- JWT authentication flow
- Error codes and handling

## Tools used

- **OpenAPI 3.0** — industry-standard API specification format
- **Redocly CLI** — rendered the spec into a hosted HTML reference page
- **GitHub Pages** — hosts the live documentation

## About the project

Aria is a personal project I built for fun — a fashion stylist powered by Mistral AI for conversational advice and Meta LLaMA 4 Vision via Groq for image analysis. It's live and fully functional. Documenting it felt like the most honest way to show what I can do: read a real codebase, understand it end to end, and produce professional documentation from scratch.
