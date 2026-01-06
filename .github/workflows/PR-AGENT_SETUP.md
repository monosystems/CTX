# PR-Agent GitHub Action Setup (OpenRouter)
# ===========================================
#
# Follow these steps to configure PR-Agent with OpenRouter:
#
# 1. Go to https://openrouter.ai/keys and create an API key
# 2. Go to your repository on GitHub
# 3. Navigate to: Settings → Secrets and variables → Actions
# 4. Click "New repository secret" and add:
#
#    Name: OPENROUTER_KEY
#    Value: sk-or-v1-...
#
# =====================================
# Available Commands
# =====================================
#
# In any PR comment, use these commands:
# - /describe - Generate PR description
# - /review - Review the PR
# - /improve - Suggest improvements
# - /ask <question> - Ask questions about the code
#
# =====================================
# Triggering Auto-Reviews
# =====================================
#
# The workflow automatically runs on:
# - PR opened
# - PR updated (new commits)
# - PR reopened
#
# For manual triggers, comment in the PR.
