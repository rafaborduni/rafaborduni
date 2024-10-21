name: Auto Close PRs

on:
  pull_request_target:
    types: [opened, reopened]

jobs:
  check_pr:
    name: Check PR
    runs-on: ubuntu-latest

    steps:
      - name: Check if employee
        id: check_employee
        uses: actions/github-script@v6
        with:
          github-token: ${{ secrets.READ_GITHUB_ORG_MEMBERS_TOKEN }}
          result-encoding: string
          script: |
            try {
              const response = await github.rest.orgs.checkMembershipForUser({
                org: 'github',  # Certifique-se de que 'github' é o nome correto da sua organização
                username: context.payload.pull_request.user.login
              });

              if (response.status === 204) {
                return 'true';
              } else {
                return 'false';
              }
            } catch (error) {
              console.error('Erro ao verificar a organização: ', error);
              return 'false';
            }

      - name: Close PR if not employee
        if: ${{ steps.check_employee.outputs.result == 'false' }}
        uses: actions/github-script@v6
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const body = 'This pull request is being automatically closed because we do not accept external contributions to this repository.';

            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.payload.pull_request.number,
              body: body
            });

            await github.rest.pulls.update({
              owner: context.repo.owner,
              repo: context.repo.repo,
              pull_number: context.payload.pull_request.number,
              state: 'closed'
            });
