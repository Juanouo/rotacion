# CLAUDE.md - AI Assistant Guide for Office Cleaning Rotation Manager

## Project Overview

This repository contains a **single-page web application** for managing office cleaning duty rotations among team members. The application is written entirely in Spanish and uses vanilla HTML, CSS, and JavaScript with no external dependencies or build process.

**Project Name**: Gestor de Rotación de Limpieza de Oficina (Office Cleaning Rotation Manager)
**Tech Stack**: HTML5, CSS3, JavaScript (ES6+), localStorage
**Language**: Spanish

## Repository Structure

```
/home/user/rotacion/
├── .git/                 # Git repository data
├── .gitignore           # Python-based gitignore (legacy, no Python files exist)
└── index.html           # Single-file application (HTML + CSS + JS)
```

### Key Points:
- **Single File Architecture**: Everything is contained in `index.html` - markup, styles, and logic
- **No Build Process**: Can be opened directly in a browser
- **No Dependencies**: Pure vanilla JavaScript, no npm/package.json
- **Client-Side Only**: No backend, all data stored in browser's localStorage

## Core Functionality

### 1. Team Member Management
- **Location**: Line 263-275 in `index.html`
- **Data Structure**: Array of colleagues with `name` and `username` properties
- **Current Team Members** (11 people):
  - Juane (jeconchao), Javi (jmpreussa), Nacho (ifaglonij), Klaus (kilehmannm)
  - Lore (lcgallardov), Hugo (hesotop), GabrielM (gemolinah), GabrielV (gavargast)
  - Javier (jaramosg), Cata (caquijadae), Victor (vaballesterosV)

### 2. Rotation Algorithm
- **Function**: `createPairs()` at line 311-325
- **Logic**:
  - Shuffles array using Fisher-Yates algorithm
  - Creates pairs from shuffled list
  - If odd number of people, last person is marked as "solo" (no cleaning duty that week)
- **Fairness**: Random shuffling ensures different pairings each rotation cycle

### 3. State Management
- **Storage**: Browser localStorage with key `'cleaningRotationState'`
- **Persisted Data**:
  - `currentRotation`: Array of pair objects with status
  - `currentWeekIndex`: Current position in rotation
  - `rotationNumber`: Cycle counter
- **Functions**:
  - `loadState()` (line 282-290): Loads state on page load
  - `saveState()` (line 293-300): Persists state to localStorage

### 4. Week Progression
- **Function**: `nextWeek()` at line 344-368
- **Workflow**:
  1. Marks current week as "completed"
  2. Advances to next week
  3. If rotation complete, prompts for new rotation
  4. Updates UI and persists state

### 5. Email Reminder Generation
- **Function**: `generateEmailContent()` at line 476-520
- **Features**:
  - Generates email preview with recipients, subject, and body
  - Calculates next Monday for scheduling
  - Different messages for pairs vs. solo assignments
  - Includes cleaning task checklist for pairs

## UI/UX Design

### Visual Theme
- **Color Scheme**: Purple gradient (primary: #667eea to #764ba2)
- **Design Style**: Modern, card-based with glassmorphism effects
- **Animations**: Pulse effect for current week, blink for status indicators

### Status System
- **waiting** (⏳): Yellow indicator, pair is scheduled for future week
- **current** (🧹): Red blinking indicator, active cleaning assignment
- **completed** (✅): Green indicator, week finished

### Responsive Layout
- Grid-based pair cards with `repeat(auto-fit, minmax(250px, 1fr))`
- Flexbox controls for button arrangement
- Mobile-friendly with proper viewport meta tag

## Development Workflows

### Making Changes to Team Members

**When adding/removing team members:**
1. Locate the `colleagues` array at line 263-275
2. Add/remove/edit entries following the format: `{ name: 'DisplayName', username: 'emailusername' }`
3. Test rotation generation to ensure pairs are created correctly
4. No other changes needed - UI updates automatically

**Example:**
```javascript
const colleagues = [
    { name: 'NewPerson', username: 'nperson' },  // Add new member
    { name: 'Juane', username: 'jeconchao' },
    // ... rest of team
];
```

### Modifying Rotation Logic

**To change pairing algorithm:**
- Edit `createPairs()` function at line 311-325
- Preserve return structure: `{ pairs: [[person1, person2], ...], solo: personOrNull }`

**To adjust week progression:**
- Modify `nextWeek()` at line 344-368
- Ensure state persistence by calling `saveState()`

### Customizing Email Templates

**To modify email content:**
- Edit `generateEmailContent()` at line 476-520
- Update subject line, greeting, or task list
- Preserve HTML formatting for email preview display

### Styling Changes

**All styles are embedded in `<style>` tag (line 7-208):**
- Color scheme: Update gradient values in `body`, `button`, `.current-week`
- Layout: Modify grid/flexbox properties in respective classes
- Animations: Edit `@keyframes pulse` (line 144-147) and `@keyframes blink` (line 204-207)

## Git Workflow & Conventions

### Branch Strategy
- **Feature branches**: Use `claude/` prefix for AI-generated branches
- **Current branch**: `claude/claude-md-mhz3dblykclsmfq8-016qvQJfCzsnrmiCYojz4r23`
- **Main branch**: Not explicitly defined in current state

### Commit Message Style
Based on git history analysis, commit messages are in Spanish and use imperative mood:
- ✅ Good: "cambiar a juane", "pasar a español", "update app rotacion"
- ✅ Keep messages concise and descriptive
- ✅ Use Spanish for consistency with codebase

### Push Protocol
- Always use: `git push -u origin <branch-name>`
- Branch names must start with `claude/` for AI assistant work
- Retry logic: Up to 4 attempts with exponential backoff (2s, 4s, 8s, 16s) on network errors

## Testing Procedures

### Manual Testing Checklist
Since this is a client-side app with no automated tests:

**1. Rotation Generation**
- [ ] Click "Generar Nueva Rotación" button
- [ ] Verify all team members appear in pairs
- [ ] Check that odd person (if any) is marked as "Solo - Sin limpieza"
- [ ] Confirm first week is marked as "current"

**2. State Persistence**
- [ ] Generate rotation and advance a few weeks
- [ ] Refresh browser page
- [ ] Verify rotation state is restored correctly
- [ ] Check rotation number and week counters match

**3. Week Progression**
- [ ] Click "Siguiente Semana" through entire rotation
- [ ] Verify status indicators change: waiting → current → completed
- [ ] Confirm completion alert appears at end
- [ ] Check buttons disable appropriately

**4. Email Preview**
- [ ] Click "Enviar Recordatorio por Email" for pair week
- [ ] Verify correct usernames in "Para:" field
- [ ] Test with solo week, check special message
- [ ] Confirm Monday date calculation is correct

**5. Reset Functionality**
- [ ] Click "Reiniciar Todo"
- [ ] Verify all state is cleared
- [ ] Check localStorage is emptied
- [ ] Confirm UI returns to initial state

## Common Modifications & Patterns

### Adding New Features

**Pattern to follow:**
1. Add UI elements in HTML section (line 210-260)
2. Create/modify JavaScript functions (line 262-526)
3. Update `updateDisplay()` function (line 381-458) for UI updates
4. Call `saveState()` if feature affects persisted state

### Localization (If translating to another language)
**Text locations to update:**
- HTML content: Headings, button labels (line 210-260)
- JavaScript strings: Alert messages, email templates (line 262-526)
- Month/date formatting: `toLocaleDateString()` calls (line 478, 500)

### Email Integration
Current implementation shows preview only. To add actual sending:
- Consider using `mailto:` links for client-side
- Or integrate with email API (requires backend)
- Update `sendEmailReminder()` function (line 460-474)

## Security & Privacy Considerations

### Data Storage
- **All data stored client-side** in localStorage
- No sensitive information (passwords, personal data) stored
- Data persists only on user's browser
- Clearing browser data removes all rotation history

### Email Addresses
- Usernames combined with `@company.com` domain
- Update domain in `generateEmailContent()` if company email changes
- No actual emails sent - preview only

### XSS Prevention
- No user input fields that render as HTML
- All dynamic content uses `textContent` or template literals
- No `eval()` or `innerHTML` with user data

## Browser Compatibility

**Tested/Recommended:**
- Modern browsers with ES6+ support (Chrome, Firefox, Safari, Edge)
- localStorage support required
- CSS Grid and Flexbox support needed

**Minimum Requirements:**
- JavaScript ES6 (for arrow functions, template literals, destructuring)
- CSS3 (gradients, animations, flexbox, grid)
- localStorage API

## Troubleshooting Guide

### Issue: Rotation state not persisting
**Solution**: Check browser localStorage settings, ensure not in private/incognito mode

### Issue: Pairs appear unbalanced
**Solution**: Verify `colleagues` array has correct number of entries, check `createPairs()` logic

### Issue: Email preview shows wrong date
**Solution**: Check system date, verify Monday calculation logic in `generateEmailContent()`

### Issue: Buttons disabled unexpectedly
**Solution**: Check `currentWeekIndex` and `currentRotation.length` values, verify state integrity

### Issue: UI not updating after state change
**Solution**: Ensure `updateDisplay()` is called after state mutations, check `saveState()` calls

## Key Code Locations Reference

| Component | Line Range | Description |
|-----------|-----------|-------------|
| Team Data | 263-275 | colleagues array with names and usernames |
| State Management | 282-300 | loadState() and saveState() functions |
| Shuffle Algorithm | 302-309 | Fisher-Yates shuffle implementation |
| Pair Creation | 311-325 | Main pairing logic with solo handling |
| New Rotation | 327-342 | generateNewRotation() function |
| Week Progression | 344-368 | nextWeek() function with completion logic |
| Reset Function | 370-379 | resetRotation() to clear all state |
| UI Update | 381-458 | updateDisplay() - main rendering function |
| Email Generation | 460-520 | Email preview and content creation |
| Initialization | 522-526 | DOMContentLoaded event listener |

## Future Enhancement Ideas

### Potential Features to Add:
1. **History Tracking**: Log past rotations for fairness verification
2. **Manual Pairing**: Allow admin to override random pairs
3. **Exclusion Rules**: Prevent certain pairs (e.g., same people twice in a row)
4. **Calendar Integration**: Export to Google Calendar/Outlook
5. **Notification System**: Browser push notifications on Monday mornings
6. **Multi-Language Support**: Add language switcher
7. **Dark Mode**: Toggle between light/dark themes
8. **Export/Import**: Backup and restore rotation data
9. **Statistics Dashboard**: Show cleaning duty distribution over time
10. **Mobile App**: PWA capabilities for offline use

### Performance Optimizations:
- Current implementation is already performant for small teams (<50 people)
- For larger teams, consider:
  - Virtual scrolling for pair grid
  - Debouncing UI updates
  - Web Workers for shuffle algorithm (if dataset grows significantly)

## AI Assistant Guidelines

### When Modifying Code:
1. **Always test manually** - no automated tests exist
2. **Preserve state management** - call `saveState()` after mutations
3. **Keep Spanish language** - maintain consistency with existing codebase
4. **Update this documentation** if adding features or changing architecture
5. **Test localStorage** - verify state persistence across page reloads
6. **Maintain single-file structure** - avoid splitting into multiple files unless explicitly requested

### When Adding Features:
1. Check if feature affects state → update saveState/loadState
2. Update `updateDisplay()` for UI changes
3. Follow existing CSS/naming conventions
4. Add buttons to controls section (line 222-229)
5. Test with 11 people (current team size) and odd numbers

### When Fixing Bugs:
1. Check browser console for JavaScript errors
2. Verify localStorage contents: `localStorage.getItem('cleaningRotationState')`
3. Test state transitions: Generate → Next Week → Complete → Reset
4. Confirm UI updates reflect state changes
5. Test edge cases: empty state, last week, solo assignments

## Questions & Support

For questions about:
- **Codebase structure**: Refer to "Repository Structure" and "Key Code Locations"
- **Functionality**: See "Core Functionality" section
- **Making changes**: Check "Development Workflows" and "Common Modifications"
- **Issues**: Consult "Troubleshooting Guide"

---

**Last Updated**: 2025-11-14
**Documentation Version**: 1.0
**Compatible with**: index.html (as of commit 58e928a)
