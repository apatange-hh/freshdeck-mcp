<general_rules>
When creating new MCP tools, always search the `src/tools/` directory first to check if a similar tool already exists before creating a new one. All tools must extend either `BaseTool` or `EnhancedBaseTool` base classes found in `src/tools/base.ts` and `src/tools/enhanced-base.ts` respectively. Use `EnhancedBaseTool` for tools that require permission-based access control.

All input validation must use Zod schemas with proper type definitions. Follow the existing patterns in tools like `src/tools/tickets.ts` for schema structure and validation.

Before committing any code, run the following quality assurance commands:
- `npm run lint` - Check code style and catch potential issues
- `npm run format` - Format code using Prettier
- `npm run typecheck` - Verify TypeScript type correctness

Maintain a minimum of 95% test coverage across all code. This is enforced by the Jest configuration and CI pipeline. Any new functionality must include comprehensive tests.

Follow the existing logging patterns using the Pino logger from `src/utils/logger.js`. Never log sensitive information like API keys or user data.

When working with API clients, use the resource-based pattern found in `src/api/resources/` rather than adding methods directly to the main client class.
</general_rules>

<repository_structure>
This repository follows a modular architecture with clear separation of concerns:

**src/api/** - Contains the main Freshdesk API client (`client.ts`) and resource-specific API implementations in the `resources/` subdirectory. The client handles authentication, rate limiting, and error handling.

**src/auth/** - Authentication and permission management system. Includes the main authenticator, permission discovery for the enhanced server, and permission definitions.

**src/core/** - Core type definitions (`types.ts`) and tool registry (`registry.ts`) used throughout the application. All shared interfaces and types are defined here.

**src/tools/** - MCP tool implementations. Contains both regular tools (e.g., `tickets.ts`) and enhanced versions (e.g., `tickets-enhanced.ts`) with permission-based access control. All tools extend base classes for consistency.

**src/utils/** - Utility functions including structured logging (`logger.ts`), error handling (`errors.ts`), and rate limiting (`rateLimiter.ts`).

**src/server/** - Enhanced server implementation with advanced features like permission discovery and tool management.

**tests/** - Comprehensive test suite organized by category: `api/`, `auth/`, `tools/`, `utils/`, `integration/`, `e2e/`, `security/`, and `mcp/`. Each category tests different aspects of the system.

**scripts/** - Development and testing utilities including coverage report generation and MCP client testing tools.

The repository supports both a standard MCP server (`src/index.ts`) and an enhanced version (`src/index-enhanced.ts`) with additional features.
</repository_structure>

<dependencies_and_installation>
This project uses npm for package management. Install dependencies with `npm install` from the root directory.

The project is built with TypeScript using a strict configuration defined in `tsconfig.json`. Build the project using `npm run build`.

**Core Dependencies:**
- `@modelcontextprotocol/sdk` - MCP protocol implementation
- `axios` - HTTP client for Freshdesk API calls
- `zod` - Runtime type validation and schema definition
- `pino` - Structured logging

**Development Dependencies:**
- `jest` with `ts-jest` - Testing framework
- `eslint` with TypeScript support - Code linting
- `prettier` - Code formatting
- `typescript` - TypeScript compiler

Environment configuration is managed through `.env` files. Copy `.env.example` to `.env` and configure your Freshdesk domain and API key before running the server.
</dependencies_and_installation>

<testing_instructions>
The project uses Jest with ts-jest preset for testing. There are two separate Jest configurations:

**Regular Tests** (`jest.config.js`) - Covers unit, integration, e2e, and security tests. Run with `npm test` or specific categories:
- `npm run test:unit` - Unit tests for individual modules
- `npm run test:integration` - Integration tests for API interactions
- `npm run test:e2e` - End-to-end workflow tests
- `npm run test:security` - Security-focused tests

**MCP Tests** (`jest.config.mcp.js`) - Tests that spawn actual MCP server processes. Run with `npm run test:mcp:all` or specific MCP test files using individual commands like `npm run test:mcp:protocol`.

All tests must maintain 95% coverage across branches, functions, lines, and statements. Coverage reports are generated automatically and can be viewed with `npm run coverage:open`.

MCP tests run sequentially (maxWorkers: 1) to avoid process conflicts and have longer timeouts due to process spawning requirements.

Test files should follow the naming convention `*.test.ts` and be placed in the appropriate category directory under `tests/`. Use the mock utilities in `tests/setup.ts` for consistent test setup.
</testing_instructions>

<pull_request_formatting>
</pull_request_formatting>
