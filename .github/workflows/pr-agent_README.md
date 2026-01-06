# PR-Agent Configuration Documentation
# ======================================
#
# This file documents how to configure PR-Agent with different LLM providers.
# For GitHub Actions, use environment variables in your workflow file instead.
#
# See: .github/workflows/pr-agent.yml
#
# ======================================
# LOCAL LLM SETUP (Ollama example)
# ======================================
#
# 1. Install Ollama: https://ollama.ai/
# 2. Pull a model: ollama pull llama2
# 3. Start Ollama server: ollama serve
# 4. Set environment variable:
#    OLLAMA_API_BASE=http://localhost:11434
#
# ======================================
# OPENAI-COMPATIBLE API (LocalAI, LM Studio)
# ======================================
#
# Many local LLM servers (LocalAI, LM Studio, etc.) provide
# an OpenAI-compatible API. Use:
#
#    OPENAI_API_BASE=http://localhost:1234/v1
#    OPENAI_KEY="not-needed-for-local"
#
# ======================================
# AVAILABLE CONFIGURATION OPTIONS
# ======================================
#
# Core settings (environment variables):
# - CONFIG_MODEL: The LLM model to use
# - CONFIG_RESPONSE_LANGUAGE: Language for PR responses (e.g., "de-DE")
#
# Tools (enable/disable):
# - CONFIG_PR_REVIEWER: true/false
# - CONFIG_DESCRIBE: true/false
# - CONFIG_IMPROVE: true/false
# - CONFIG_ASK: true/false
#
# For full documentation, see:
# https://qodo-merge-docs.qodo.ai/
