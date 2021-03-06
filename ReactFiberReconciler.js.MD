# ReactFiberReconciler.js

## Table of Contents
- [createContainer](#createContainer)
- [updateContainer](#updateContainer)
- [scheduleTopLevelUpdate](#scheduleTopLevelUpdate)

<a name="createContainer"></a>
## createContainer
```
function createContainer(containerInfo) {
  return ReactFiberRoot.createFiberRoot(containerInfo)
}
```

<a name="updateContainer"></a>
## updateContainer
#### what it looks like
```
updateContainer(
  element: ReactNodeList,
  container: OpaqueRoot,
  parentComponent: ?ReactComponent,
  callback: ?Function,
)
```
#### what it does
Retrieves context from `parentComponent` and sets `container.context` or `container.pendingContext`. It then calls [`scheduleTopLevelUpdate`](#scheduleTopLevelUpdate) to update the container's fiber.
```
const context = getContextForSubtree(parentComponent);
if(container.context === null) {
  container.context = context;
} else {
  container.pendingContext = context;
}

const current = container.current;
scheduleTopLevelUpdate(current, element, callback);
```

<a name="scheduleTopLevelUpdate"></a>
## scheduleTopLevelUpdate
#### what it looks like
```
scheduleTopLevelUpdate(
  current: Fiber,
  element: ReactNodeList,
  callback: ?Function,
)
```
#### what it does
Determines the priority level of this element's pending update, then schedules the update with the determined priority level.

See [`scheduleUpdate()`](ReactFiberScheduler.js.MD#scheduleUpdate)
```
const forceAsync = //TRUE if element is an async wrapper component. This results in LowPriority.
const priorityLevel = ReactFiberScheduler.getPriorityContext(current, forceAsync)

const nextState = {element};

ReactFiberUpdateQueue.addTopLevelUpdate(current, nextState, callback, priorityLevel);
scheduleUpdate(current, priorityLevel);
```
