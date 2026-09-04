# AI Bureaucracy Navigator integration kit

These are independent React TypeScript page modules. They intentionally use semantic HTML and CSS class names only, so your existing layout, buttons, modal system, typography, and tokens can style them.

Copy the `integration-kit` folder into your app, then add routes for `LifeEventWizard`, `SchemeRecommender`, and `KnowledgeHub` using your existing router and navigation framework.

## Connect existing systems

Pass the existing app's actions as props. Every integration point is optional, so the pages remain usable while you wire services one at a time.

```tsx
<KnowledgeHub
  addToRoadmap={(service) => roadmapStore.addService(service)}
  addToChecklist={(documents, context) => checklistStore.addMany(documents, context)}
  addToTracker={(service) => trackerStore.createFromService(service)}
  scheduleNotification={(service) => notificationStore.createReminder(service)}
  openOfficialUrl={(url) => window.open(url, "_blank", "noopener,noreferrer")}
  askAssistant={(prompt) => assistant.openWithPrompt(prompt)}
/>
```

Use the same callbacks for the other two pages. `LifeEventWizard` also accepts `onOpenKnowledge(articleId)`; route this to the Knowledge Hub with the article ID in route state or a query parameter. `SchemeRecommender` accepts `onViewScheme(scheme)`, which can open your existing modal or detail route.

## Navigation labels

Add these items to your existing sidebar or header:

- `Life Events`
- `Schemes`
- `Knowledge Hub`

## Data and trust

Replace `data/seedData.ts` with your backend/RAG repositories when available. The included records are examples, not legal or official eligibility decisions. Preserve the visible potential-eligibility disclaimer and last-verified labels when connecting live data.

## Suggested route hand-off

For the Life Events -> Knowledge Hub path, store the requested article ID in your router, then render:

```tsx
<KnowledgeHub initialArticleId={routeState.articleId} {...navigatorActions} />
```

This preserves the user journey without embedding a second navigation system in the module.
