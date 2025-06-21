# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Building and Testing
- **Build the package**: `swift build`
- **Run all tests**: `swift test`
- **Run a specific test**: `swift test --filter <TestClassName>.<testMethodName>`
- **Build documentation**: Swift Package Index builds documentation automatically (configured in `.spi.yml`)

### Package Management
- **Update dependencies**: `swift package update`
- **Show package dependencies**: `swift package show-dependencies`

## Architecture Overview

AIProxySwift is a Swift package that provides a unified client library for multiple AI service providers. The architecture follows a dual-service pattern:

### Core Architecture Patterns

1. **Dual Service Pattern**: Each AI provider has two service implementations:
   - `DirectService`: Makes requests directly to the provider's API (for prototyping/BYOK)
   - `ProxiedService`: Routes requests through AIProxy backend for security (production)

2. **Service Hierarchy**:
   - `ServiceMixin`: Base protocol providing common HTTP request/response handling
   - `DirectService`: Protocol for direct provider access
   - `ProxiedService`: Protocol for AIProxy-mediated access
   - Provider-specific services (e.g., `OpenAIService`, `AnthropicService`)

3. **Request/Response Models**: Each provider has dedicated:
   - Request body types (e.g., `OpenAIChatCompletionRequestBody`)
   - Response body types (e.g., `OpenAIChatCompletionResponseBody`)
   - Streaming chunk types for real-time responses

### Supported AI Providers
OpenAI, Anthropic, Gemini, Stability AI, DeepL, Together AI, Replicate, ElevenLabs, Fal, Groq, Perplexity, Mistral, EachAI, OpenRouter, DeepSeek, Fireworks AI, Brave

### Key Components

- **AIProxy**: Main configuration class and SDK entry point
- **BackgroundNetworker**: Handles HTTP networking operations
- **RealtimeActor**: Manages WebSocket connections for real-time features
- **Audio components**: `AudioController`, `AudioPCMPlayer`, `MicrophonePCMSampleVendor` for audio processing
- **Security**: Certificate pinning, DeviceCheck verification, anonymous account management

### Platform Support
- iOS 15+, macOS 13+, visionOS 1+, watchOS 9+
- Swift 5.9+ (Swift Package Manager)

### Testing
- Uses Swift Testing framework (not XCTest)
- Test files in `Tests/AIProxyTests/` with comprehensive coverage of request/response models
- End-to-end tests for OpenAI integration

### Security Features
The library implements multiple security layers when using ProxiedService:
- Certificate pinning via `AIProxyCertificatePinning`
- DeviceCheck verification via `AIProxyDeviceCheck`
- Anonymous account management in `AnonymousAccount/` folder
- Keychain storage for secure credential management