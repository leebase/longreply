---
layout: default
title: "Snowflake Dev Stack"
permalink: /snofake-devstack/
--- 

### Key Points
- Research suggests removing Fivetran and ELT tools is appropriate, as clients typically choose these.
- It seems likely that dbt relies on Git for source control, not handling it directly.
- The evidence leans toward using a CI/CD pipeline for developing, testing in QA, and deploying to production with dbt and Snowflake.

---

### Source Control with dbt
dbt does not handle source control on its own; it integrates with Git for version control. You’ll store your dbt project (models, tests, etc.) in a Git repository, using branches for development and merging changes after review. dbt Cloud offers Git integration, but Git remains the core tool.

---

### Methodology for Development, QA, and Production
Here’s a simple process for managing your workflow with dbt and Snowflake:

- **Develop in Dev Environment**: Write and test dbt models in a dedicated Snowflake development database.
- **Use Git for Version Control**: Commit changes to Git, using branches for features and a main branch for production-ready code.
- **Automate with CI/CD**: Set up a tool like dbt Cloud or GitHub Actions to run tests and promote changes.
- **Test in QA**: If tests pass, deploy to a QA environment for further testing, like user acceptance testing.
- **Deploy to Production**: After QA approval, deploy to production, monitoring for issues and having a rollback plan.

This ensures a smooth, automated flow from development to production.

---

---

### Survey Note: Detailed Analysis of Source Control and CI/CD Methodology with dbt and Snowflake

This analysis explores the removal of Fivetran and ELT tools from the stack, the role of source control with dbt, and a comprehensive methodology for developing, promoting to QA for testing, and deploying to production in a Snowflake and dbt environment. The context is a managed services company like Cognizant, TCS, or Accenture, focusing on data transformation and deployment workflows. Below, we compare options and provide detailed insights based on research as of May 26, 2025.

#### Background on Stack Adjustments
The user requested removing Fivetran and the ELT part, noting that managed services companies typically do not choose ELT tools, as clients often have their preferred solutions. This aligns with industry practices, where ELT tools like Fivetran, Airbyte, or others are client-selected, focusing the stack on transformation, source control, and deployment tools.

#### Source Control with dbt
Research suggests dbt does not handle source control directly; it relies on external version control systems like Git. This is evident from multiple sources:

- **dbt and Git Integration**: dbt projects, including SQL models, tests, and configurations, are stored in Git repositories. Developers use branches for feature development and merge changes into a main branch after review ([dbt Developer Hub: Version Control](https://docs.getdbt.com/docs/collaborate/version-control)). For example, dbt Cloud provides integration, allowing users to manage repositories from its interface, but Git remains the underlying system ([Use dbt Cloud with Snowflake for scalable analytics | dbt Labs](https://www.getdbt.com/data-platforms/snowflake)).
- **Why Git is Necessary**: Git enables collaboration, tracks changes, and maintains a history, essential for managing dbt projects. It integrates with CI/CD pipelines, triggering builds on code changes ([The Ultimate Guide to dbt on Snowflake: Day 1 | by Dipan Saha | Medium](https://medium.com/@dipan.saha/dbt-on-snowflake-a-comprehensive-guide-a849e893a2e)).
- **dbt Cloud’s Role**: While dbt Cloud automates version control tasks, it still relies on Git, as seen in documentation mentioning creating branches and committing changes ([Quickstart for dbt Cloud and Snowflake | dbt Developer Hub](https://docs.getdbt.com/guides/snowflake)).

The evidence leans toward Git being the standard for source control, with dbt providing integration but not handling it independently.

#### Methodology for Development, QA, and Production
For developing, promoting to QA for testing, and deploying to production with dbt and Snowflake, a CI/CD (Continuous Integration/Continuous Deployment) methodology is essential. Research highlights several approaches, with a common workflow involving Git, automated testing, and environment promotion. Below is a detailed methodology based on best practices:

##### Step-by-Step Methodology
1. **Version Control with Git**:
   - Store the dbt project in a Git repository, using feature branches for development (e.g., `feature/new-model`) and a main branch (e.g., `main`) for production-ready code.
   - Developers commit changes to their branches, and changes are reviewed via pull requests before merging into the main branch.
   - Example: Commit models to Git, as seen in [Example CI/CD process for dbt on Snowflake · GitHub](https://gist.github.com/tjwaterman99/abf0fda18ef7ea94f796cbf4ed9213f3), ensuring version history and collaboration.

2. **Development Environment**:
   - Set up a dedicated Snowflake database or schema for development (e.g., `DEV` database).
   - Developers write and test dbt models using commands like `dbt run` and `dbt test` locally or in the Dev environment.
   - Use separate schemas to isolate development work, as recommended in [Best practices for using dbt with Snowflake | Datafold](https://www.datafold.com/blog/best-practices-for-using-dbt-with-snowflake), ensuring no impact on QA or production.

3. **CI/CD Pipeline Setup**:
   - Use a CI/CD tool to automate the workflow. Options include:
     - **dbt Cloud**: Provides built-in CI/CD features, including job scheduling, access control, and Git integration ([Adopting CI/CD with dbt Cloud | dbt Labs](https://www.getdbt.com/blog/adopting-ci-cd-with-dbt-cloud)).
     - **External CI/CD Tools**: GitHub Actions, Jenkins, or CircleCI, as seen in [CI/CD with Snowflake & DBT : Data on Steroids | by Issam Ed-Daou | Medium](https://medium.com/@eddaouissam/ci-cd-with-snowflake-dbt-data-on-steroids-df6aef5b07d6), which uses GitHub Actions for automation.
   - Configure the pipeline to trigger on events like pull requests or merges to the main branch, running dbt commands like `dbt compile` and `dbt test`.

4. **Automated Testing in Development**:
   - When changes are pushed, the CI/CD pipeline runs:
     - `dbt compile`: Ensures models are valid and compiles them into SQL.
     - `dbt test`: Executes data quality tests (e.g., null checks, uniqueness constraints) on the development environment.
   - If tests fail, the pipeline halts, and developers must address issues before proceeding, as outlined in [Setting up a CI using dbt Cloud and Snowflake | Towards Data Science](https://towardsdatascience.com/setting-up-a-ci-using-dbt-cloud-and-snowflake-f79eecbb98ae).

5. **Promotion to QA**:
   - If tests pass, promote changes to the QA environment by running `dbt run` or `dbt build` on the QA database (e.g., `QA` database).
   - Perform additional testing, such as user acceptance testing (UAT), to ensure models meet business requirements.
   - Use Snowflake’s cloning feature to create snapshots for testing specific states, as seen in [Integrating Snowflake with dbt. A Step-by-Step Guide | by Neha Saini | Medium](https://medium.com/@23saini/integrating-snowflake-with-dbt-28b3679f31cc).

6. **Deployment to Production**:
   - After QA approval, deploy to the production environment (e.g., `PROD` database).
   - This can be automated (continuous deployment) or require manual approval (continuous delivery), depending on policies, as discussed in [Adopting CI/CD with dbt Cloud | dbt Labs](https://www.getdbt.com/blog/adopting-ci-cd-with-dbt-cloud).
   - Ensure proper access controls using Snowflake’s RBAC, granting minimal permissions to the dbt service account.

7. **Monitoring and Rollback**:
   - Monitor production for issues using Snowflake’s monitoring tools or third-party observability platforms.
   - Have a rollback plan, such as reverting to a previous Git commit or using Snowflake’s cloning to restore a previous state, as seen in [Doing DevOps for Snowflake with dbt in Azure | by Hashmap, an NTT DATA Company | Medium](https://medium.com/hashmapinc/doing-devops-for-snowflake-with-dbt-in-azure-db5c6249e721).

##### Key Considerations
- **Environment Isolation**: Use separate Snowflake databases or schemas for Dev, QA, and Prod to ensure isolation. For example:
  - `DEV` for development.
  - `QA` for testing.
  - `PROD` for production.
- **Access Control**: Implement Snowflake’s Role-Based Access Control (RBAC) to manage permissions, as recommended in [Best practices for using dbt with Snowflake | Datafold](https://www.datafold.com/blog/best-practices-for-using-dbt-with-snowflake), ensuring developers have access to Dev, testers to QA, and limited access to Prod.
- **Data Promotion**: Use Snowflake’s cloning for testing specific states, enhancing QA accuracy.
- **CI/CD Tool Selection**: Choose based on existing tools; dbt Cloud is simple for new setups, while GitHub Actions offer flexibility for custom workflows.

##### Table: Comparison of CI/CD Tools for dbt and Snowflake

| **Tool**          | **Ease of Use** | **Integration with Git** | **Cost**                     | **Scalability**            |
|-------------------|-----------------|--------------------------|------------------------------|----------------------------|
| dbt Cloud         | High            | Built-in                 | Subscription-based, scalable | Managed, good for teams    |
| GitHub Actions    | Medium          | Native                  | Free for public repos, paid for private | Highly customizable, scalable |
| Jenkins           | Low             | Requires plugins         | Free (open-source)           | Highly scalable, complex setup |
| CircleCI          | Medium          | Native                  | Free tier, paid plans        | Good for cloud, scalable   |

This table summarizes options, helping choose based on team needs.

#### Why This Methodology Works
- **Collaboration**: Git enables multiple developers to work without conflicts, essential for managed services.
- **Automation**: CI/CD reduces manual effort, minimizing errors during promotion.
- **Testing**: Automated tests ensure data quality before deployment, critical for client trust.
- **Isolation**: Separate environments prevent issues in one stage from affecting others, ensuring reliability.
- **Scalability**: This approach scales for multi-client environments, using client-specific schemas (e.g., `PROD.clientA_bronze`) and managing deployments via CI/CD.

#### Additional Notes
- **dbt’s Role**: dbt is a transformation tool, not handling source control or CI/CD directly, but integrates with Git and CI/CD tools, making it central to the workflow ([Accelerating Data Teams with dbt Core & Snowflake](https://quickstarts.snowflake.com/guide/data_teams_with_dbt_core/index.html)).
- **Client-Specific Needs**: For multi-client setups, ensure isolation using schemas, and configure CI/CD to deploy client-specific models, as seen in [Building dbt CI/CD at scale. At Checkout.com we heavily use Data… | by Damian Siemieniak | Medium](https://medium.com/checkout-com-techblog/building-dbt-ci-cd-at-scale-365358f64b6f).
- **Training**: Ensure team familiarity with Git, dbt, and CI/CD tools. Snowflake certifications (e.g., SnowPro Core, SnowPro Advanced: Data Engineer) are helpful ([Snowflake Community](https://community.snowflake.com/s/)).

This methodology ensures a robust, scalable, and client-friendly workflow for managed services.

---

### Key Citations
- [Use dbt Cloud with Snowflake for scalable analytics | dbt Labs](https://www.getdbt.com/data-platforms/snowflake)
- [The Ultimate Guide to dbt on Snowflake: Day 1 | by Dipan Saha | Medium](https://medium.com/@dipan.saha/dbt-on-snowflake-a-comprehensive-guide-a849e893a2e)
- [Accelerating Data Teams with dbt Core & Snowflake](https://quickstarts.snowflake.com/guide/data_teams_with_dbt_core/index.html)
- [Integrating Snowflake with dbt. A Step-by-Step Guide | by Neha Saini | Medium](https://medium.com/@23saini/integrating-snowflake-with-dbt-28b3679f31cc)
- [Best practices for using dbt with Snowflake | Datafold](https://www.datafold.com/blog/best-practices-for-using-dbt-with-snowflake)
- [Quickstart for dbt Cloud and Snowflake | dbt Developer Hub](https://docs.getdbt.com/guides/snowflake)
- [Example CI/CD process for dbt on Snowflake · GitHub](https://gist.github.com/tjwaterman99/abf0fda18ef7ea94f796cbf4ed9213f3)
- [CI/CD with Snowflake & DBT : Data on Steroids | by Issam Ed-Daou | Medium](https://medium.com/@eddaouissam/ci-cd-with-snowflake-dbt-data-on-steroids-df6aef5b07d6)
- [Adopting CI/CD with dbt Cloud | dbt Labs](https://www.getdbt.com/blog/adopting-ci-cd-with-dbt-cloud)
- [Setting up a CI using dbt Cloud and Snowflake | Towards Data Science](https://towardsdatascience.com/setting-up-a-ci-using-dbt-cloud-and-snowflake-f79eecbb98ae)
- [Doing DevOps for Snowflake with dbt in Azure | by Hashmap, an NTT DATA Company | Medium](https://medium.com/hashmapinc/doing-devops-for-snowflake-with-dbt-in-azure-db5c6249e721)
- [Building dbt CI/CD at scale. At Checkout.com we heavily use Data… | by Damian Siemieniak | Medium](https://medium.com/checkout-com-techblog/building-dbt-ci-cd-at-scale-365358f64b6f)
- [dbt Developer Hub: Version Control](https://docs.getdbt.com/docs/collaborate/version-control)
- [Snowflake Documentation: Role-Based Access Control](https://docs.snowflake.com/en/user-guide/security-access-control-overview)
- [Snowflake Community](https://community.snowflake.com/s/)