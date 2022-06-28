# dbdocs-action
This Github Action is used for the dynamic creation of an Entity Relationship Diagram from a dbml file.
The dbml file is required for the dbdocs.io commands.

To use the Action in a repository, the first thing that you have to do is generate your token access.
You can generate this token and use it on github secrets or the another way that you want.

After the token generation you need just include the Action called in the workflow file (.github/workflows) as shown in the example below:

```yaml
- name: Trybe DBdocs
  if: github.event_name == 'pull_request'
  uses: dbdocs-action@main
  with:
    token: ${{ secrets.DBDOCS_TOKEN }} 
    filename: "dbml-file.dbml"
```

The created URL and password will be available within the workflow where the action was added.
You can also use the [actions/github-script@0.3.0](https://github.com/actions/github-script) action to make the information appear in the Pull Request. Example:

```yaml
- name: "Comment PR"
        uses: actions/github-script@0.3.0
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const message = `
              :steam_locomotive: Your preview app for ${{ github.sha }} is ready!
              DBDOCS URL: ${{process.env.DBDOCS_URL}}
              DBDOCS PASSWORD: ${{process.env.DBDOCS_PASSWORD}}
              Check it out! :shipit:
            `;
            const { issue: { number: issue_number }, repo: { owner, repo }  } = context;
            github.issues.createComment({ issue_number, owner, repo, body: message });
```

For more information access: https://dbdocs.io/docs