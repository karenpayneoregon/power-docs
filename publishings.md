# Instructions

- [Write agent instructions](https://learn.microsoft.com/en-us/microsoft-copilot-studio/authoring-instructions)
- [Agent evaluation](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/analytics-agent-evaluation-intro)
- [Key concepts](https://learn.microsoft.com/en-us/microsoft-copilot-studio/publication-fundamentals-publish-channels?tabs=web)
- [Agents for customer engagement and handoff](https://learn.microsoft.com/en-us/microsoft-copilot-studio/customer-copilot-overview)
- Publishing to channels
  - [Publish to web](https://learn.microsoft.com/en-us/microsoft-copilot-studio/publication-fundamentals-publish-channels?tabs=web)
  - [Publish to Microsoft Teams](https://learn.microsoft.com/en-us/microsoft-copilot-studio/publication-add-bot-to-microsoft-teams)
    - [How To Deploy Copilot Studio Agents in Microsoft Teams](https://www.youtube.com/watch?v=NpSsxmbhGQU) YouTube :film_strip:
    - Copilot Studio Publishing to [Channels Quick Guide](https://learn.microsoft.com/en-us/microsoft-copilot-studio/publication-add-bot-to-microsoft-teams) V.1

## How to remove Channel Agent

- [Uninstall agents](https://support.microsoft.com/en-us/microsoft-365-copilot/uninstall-agents-in-the-microsoft-365-copilot-app) in the Microsoft 365 Copilot app

---

As a Microsoft 365 admin, treat a Copilot agent in Teams like a Teams/Microsoft 365 app. You usually **remove availability**, **uninstall it for users if it was deployed**, and optionally **delete it from the org catalog** if it is a custom app.

## Option 1: Remove/block the agent in Microsoft 365 admin center

1. Go to **Microsoft 365 admin center**.
2. Open **Copilot** or **Settings > Integrated apps**, depending on your tenant experience.
3. Find the Copilot agent.
4. Select the agent.
5. Choose **Remove**, **Block**, or adjust availability so users no longer have access.

Microsoft’s current guidance is that agents can be managed through the Microsoft 365 admin center, including enabling, assigning, blocking, and removing agents. ([Microsoft Learn][1])

## Option 2: Block it in Teams admin center

1. Go to **Teams admin center**.
2. Open **Teams apps > Manage apps**.
3. Search for the agent/app name.
4. Select it.
5. Choose **Block**.

This prevents users from using the agent in Teams. Microsoft documents that the Teams **Manage apps** page shows available agents and apps, and admins can allow or block them there. ([Microsoft Learn][2])

## Option 3: Remove it from setup policies

Do this if the agent was pushed or pinned to users.

1. In **Teams admin center**, go to **Teams apps > Setup policies**.
2. Open the policy that deployed or pinned the agent.
3. Remove the agent from:

   * **Installed apps**
   * **Pinned apps**
4. Save the policy.

Setup policies are used to install and pin apps/agents for users in Teams. Removing the agent from the policy stops it from being deployed that way. ([Microsoft Learn][3])

## Option 4: Delete a custom agent/app from the Teams org catalog

Use this only if it is a custom Teams app/agent uploaded to your organization.

1. Go to **Teams admin center**.
2. Open **Teams apps > Manage apps**.
3. Find the custom app/agent.
4. Open it.
5. Delete/remove it from the organization catalog if available.

Microsoft’s Teams custom app management docs cover deleting custom apps from the organization catalog. ([Microsoft Learn][4])

## Important catch

Blocking the app does **not always instantly remove the tile from every user’s Teams client**. It mainly prevents use and future access. If the agent was installed through policy, remove it from the setup policy too. If users manually installed it, they may still see stale entries until Teams refreshes, signs out/in, or the client cache updates.

For a clean admin-side removal, I would do this order:

1. **Remove/block the agent in Microsoft 365 admin center**.
2. **Block it in Teams admin center > Manage apps**.
3. **Remove it from Teams app setup policies**.
4. **Delete it from the custom app catalog** if it was uploaded as a custom app.
5. Wait for propagation, then have affected users restart Teams.

[1]: https://learn.microsoft.com/en-us/microsoft-365/admin/manage/manage-copilot-agents-integrated-apps?view=o365-worldwide&utm_source=chatgpt.com "Manage agents in the Microsoft 365 admin center"
[2]: https://learn.microsoft.com/en-us/microsoftteams/manage-apps?utm_source=chatgpt.com "Manage your apps in the Microsoft Teams admin center - Microsoft Teams ..."
[3]: https://learn.microsoft.com/en-us/microsoftteams/teams-app-setup-policies?utm_source=chatgpt.com "Manage agents and app setup policies in Microsoft Teams - Microsoft ..."
[4]: https://learn.microsoft.com/en-us/microsoftteams/teams-custom-app-policies-and-settings?utm_source=chatgpt.com "Manage custom app policies and settings - Microsoft Teams"
