---
name: localstorage-autosync
description: Version-based smart merge system for web apps using localStorage. Automatically syncs source data changes (prices, content, config) to users on refresh while preserving user-editable fields (preferences, approvals, status). Use when working with single-file web apps or simple builds that persist data to localStorage and need code changes to propagate to users without requiring manual cache/data reset. Triggers include "add auto-sync", "localStorage not updating", "users seeing stale data", or "version-based data sync".
---

# localStorage Auto-Sync

Implements version-based smart merge for localStorage-backed web apps. Source data updates automatically sync to users while preserving their edits.

## When to Use

- Single-file apps or simple builds using localStorage for persistence
- Users are seeing stale/cached data after code updates
- No backend database (Supabase, Firebase, etc.) - if using a database, data should live there instead

## When NOT to Use

- App already uses a database for persistence (the database IS the source of truth)
- App doesn't persist data to localStorage
- Build system already handles cache-busting/versioning

## Implementation

### 1. Add Version Constant

Place near the top of the script, before data definitions:

```javascript
// Increment when INITIAL_DATA changes to trigger smart merge
const DATA_VERSION = 'YYYY-MM-DD-1';
```

### 2. Define User-Editable Fields

Specify which fields users can modify (preserved during merge). Everything else syncs from source:

```javascript
const USER_EDITABLE_FIELDS = {
  items: ['status', 'userNotes'],      // Example: user can change status
  settings: ['theme', 'preferences'],   // Example: user preferences
  // Add categories as needed for the app
};
```

### 3. Add Smart Merge Function

```javascript
const smartMergeData = (savedData, sourceData) => {
  const merged = {};
  for (const category of Object.keys(sourceData)) {
    const sourceItems = sourceData[category];
    const savedItems = savedData[category] || [];
    const userFields = USER_EDITABLE_FIELDS[category] || [];

    merged[category] = sourceItems.map(sourceItem => {
      const savedItem = savedItems.find(s => s.id === sourceItem.id);
      if (savedItem) {
        // Preserve user fields, update everything else from source
        const mergedItem = { ...sourceItem };
        userFields.forEach(field => {
          if (savedItem[field] !== undefined) {
            mergedItem[field] = savedItem[field];
          }
        });
        return mergedItem;
      }
      return sourceItem; // New item from source
    });
  }
  return merged;
};
```

### 4. Update Load Logic

Modify the data loading to check version and merge if needed:

```javascript
// On load
const saved = localStorage.getItem('app-data');
const savedVersion = localStorage.getItem('app-version');

if (saved) {
  const parsed = JSON.parse(saved);

  if (savedVersion !== DATA_VERSION) {
    // Version mismatch - smart merge
    console.log(`Data updated: ${savedVersion || 'none'} → ${DATA_VERSION}`);
    const mergedData = smartMergeData(parsed, INITIAL_DATA);
    setData(mergedData);
    localStorage.setItem('app-version', DATA_VERSION);
  } else {
    // Version matches - use saved data
    setData(parsed);
  }
} else {
  // No saved data - set initial version
  localStorage.setItem('app-version', DATA_VERSION);
}
```

## Adapt to Project

- **Match code style**: Vanilla JS vs React hooks vs Vue, etc.
- **Identify user fields**: Look at what users actually edit in the UI
- **Storage key naming**: Use the app's existing localStorage key pattern
- **Version format**: `YYYY-MM-DD-N` recommended, but any incrementing string works

## Workflow

1. Assess if pattern fits (localStorage-backed, no database, stale data problem)
2. Identify user-editable vs source-controlled fields
3. Implement the four components above
4. Bump `DATA_VERSION` whenever source data changes
5. Test by checking console for version update message on refresh
