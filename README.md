# Release Backport Action

Action to create a backport PR automatically and notify developers via Slack:

![slack notification](https://user-images.githubusercontent.com/5386489/176590743-c0d25899-9bfc-4ad2-9e8a-cd6d656a02b7.png)

## Usage

### Example

```yml
# Release workflow
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    # ... some steps or jobs doing release
    - name: Create Github release
      id: create_github_release
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      uses: release-drafter/release-drafter@v5
      with:
        version: ${{ env.RELEASE_VERSION }}
        tag: ${{ env.RELEASE_VERSION }}
        publish: true
    - name: Backport
      uses: iCHEF/release-backport-action@v1
      with:
        repo-name: ${{ env.GITHUB_REPOSITORY }}
        github-token: ${{ secrets.GITHUB_TOKEN }}
        release-name: ${{ env.RELEASE_VERSION }}
        release-info: ${{ steps.create_github_release.outputs.html_url }}
        slack-bot-token: ${{ secrets.SLACK_BOT_TOKEN }}
        slack-notify-channel-id: ${{ secrets.SLACK_CHANNEL_ID }}
```

### Inputs

```yml
inputs:
  github-token:
    description: 'Github token for creating Pull Request'
    required: true
    default: ''
  release-name:
    description: 'Release name for backport branch name. Usually is a version number.'
    required: true
    default: ''
  release-info:
    description: 'The info of this release. Usually is a GitHub release URL.'
    required: true
    default: ''
  slack-bot-token:
    description: 'The slack bot token for notifying.'
    required: true
    default: ''
  slack-notify-channel-id:
    description: 'The target channel id for notifying.'
    required: true
    default: ''
  pr-destination-branch:
    description: 'The branch which needs backport.'
    required: false
    default: 'develop'
```
