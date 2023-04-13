Every ***Tuesday at 7 pm pst*** a **maintenance** deploy to production is done. If a hotfix is needed on another date, the process described on this page is still valid.

The workflow is as follows:

1. **Source** ([Repository](https://github.com/gitMLS/etl/))
    - *API deploy*
2. **Manual Approval** ([Pipeline](https://us-east-2.console.aws.amazon.com/codesuite/codepipeline/pipelines/docker-production/view?region=us-east-2))
3. **Building process**

# Source

**For maintenance deployment**:

1.  First, create a revision branch from the master. Follow the below naming pattern:

    `maintenance-{-YYYY-mm-dd}`
    
    Use your preferred tool. For example, the GitHub web console:

    ![alt text](https://drive.google.com/uc?id=1lgX7Hgu3FRIjWmMDs0t0nA5ASlWbHScG)

2. Share the created branch with the team on Slack.

    Private Channel: production-deployment

    ![alt text](https://drive.google.com/uc?id=1m0P1KT3biYdvobK48YYNO32WXkjApQ1d)

3. ETL team creates a pull request to the revision branch

4. Merge ETL pull request into the revision branch

5. Merge the revision branch into the master branch

**Hotfix integration**

Hotfix integrations should follow the steps described for maintenance, except that the name pattern should be as follow:
    `hotfix-{-YYYY-mm-dd}-{fix_name}`

## API Deploy

Any needed DML or DDL statement needed for the API should be done on ***db.findbuyers.com***. Ask for credentials and schema details from the appropriate users.

# Manual approval

The triggered pipeline will be [docker-production](https://us-east-2.console.aws.amazon.com/codesuite/codepipeline/pipelines/docker-production/view?region=us-east-2), which requires manual approval from an authorized user.

![alt text](https://drive.google.com/uc?id=1nmiGhlwI1dZAU3iYXUMpMtSoakndunkk)

# Building process

Keep the rest of the team informed about the building process state. Any error due to infrastructure issues (quotas, missing resources, etc), needs for retry and hotfix integrations, should be managed by an IAM user with enough permissions on the [pipeline](https://us-east-2.console.aws.amazon.com/codesuite/codepipeline/pipelines/docker-production/view?region=us-east-2) and the [repository](https://github.com/gitMLS/etl/).