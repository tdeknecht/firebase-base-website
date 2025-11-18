# Prompt Library Reorganization Plan

**Date**: 2025-11-18
**Objective**: Separate platform-agnostic principles from Firebase-specific implementations to create a reusable, technology-neutral prompt library

---

## Problem Statement

The current `.prompts/` directory contains a mix of:
- Universal software development principles
- Firebase-specific implementations
- Mixed content without clear separation

This makes it difficult to:
- Reuse prompts across different hosting platforms
- Identify which guidance is universal vs platform-specific
- Maintain consistency between agnostic principles and Firebase implementations

---

## Analysis of Current Prompts

### Firebase-Specific Prompts
**Criteria**: Title includes "Firebase" or content exclusively uses Firebase APIs/services

1. **`firebase-best-practices.md`** ✅ KEEP AS-IS
   - Clearly labeled as Firebase-specific
   - Contains Firebase SDK patterns, Firestore security rules, Firebase Auth flows
   - Should remain in Firebase-specific category

2. **`deployment-cicd.md`** 🔄 NEEDS REFACTORING
   - Title: "Deployment & CI/CD Prompt for Firebase Applications"
   - Heavily Firebase-focused (Firebase Hosting, GitHub Actions with Firebase CLI)
   - Contains universal CI/CD principles mixed with Firebase implementation

3. **`monitoring-observability.md`** 🔄 NEEDS REFACTORING
   - Title: "Monitoring & Observability Prompt for Firebase Applications"
   - Uses Firebase Performance Monitoring, Firebase Analytics APIs
   - Contains universal observability principles

4. **`platform-simplification.md`** 🔄 NEEDS REFACTORING
   - Focuses on "Firebase + GitHub" single-platform strategy
   - Compares Firebase Hosting vs Vercel
   - Universal principle: platform simplification; Implementation: Firebase-specific

5. **`finops-free-tier-maximization.md`** ✅ FIREBASE-SPECIFIC
   - Entirely focused on Firebase free tier limits and optimization
   - Contains Firebase-specific quotas, Firestore optimization patterns
   - Should remain in Firebase-specific category

6. **`budget-resilience.md`** 🔄 NEEDS REFACTORING
   - Title: "Budget-Friendly Resilience Prompt for Firebase Applications"
   - Error handling patterns are universal; Firebase resilience patterns are specific
   - References Firebase services for resilience architecture

---

### Platform-Agnostic Prompts
**Criteria**: Content applies to any technology stack or hosting platform

1. **`code-structure.md`** ✅ UNIVERSAL
   - Universal architecture patterns (separation of concerns, dependency injection)
   - No platform-specific content
   - Should remain in core/universal category

2. **`modular-architecture-principles.md`** ✅ UNIVERSAL
   - Universal modularity concepts (right-sized modules, coupling/cohesion)
   - No platform-specific content
   - Should remain in core/universal category

3. **`feature-extensibility.md`** ✅ UNIVERSAL
   - Universal extensibility patterns (plugin architecture, strategy pattern)
   - No platform-specific content
   - Should remain in core/universal category

4. **`asset-reusability.md`** ✅ UNIVERSAL
   - Universal asset management patterns (DRY principles, asset pipeline)
   - No platform-specific content
   - Should remain in core/universal category

5. **`git-best-practices.md`** ✅ UNIVERSAL
   - Universal Git workflows, commit conventions, branching strategies
   - No platform-specific content
   - Should remain in core/universal category

6. **`prompt-maintenance.md`** ✅ META-GUIDANCE
   - Meta-prompt about maintaining the prompt library itself
   - No platform-specific content
   - Special category: library maintenance

---

### Mixed Prompts (Require Separation)
**Criteria**: Contains both universal principles and platform-specific implementations

1. **`testing-qa.md`** 🔄 LIKELY MIXED
   - Testing principles (unit, integration, e2e) are universal
   - Firebase Emulator testing, Firestore security rules testing are Firebase-specific
   - **Action**: Split into universal testing principles + Firebase testing implementation

2. **`security-architecture.md`** 🔄 LIKELY MIXED
   - Security principles (defense in depth, least privilege, input validation) are universal
   - Firebase security rules, Firebase Auth patterns are Firebase-specific
   - **Action**: Split into universal security principles + Firebase security implementation

---

## Proposed Reorganization Structure

### Option 1: Folder-Based Organization (RECOMMENDED)

```
.prompts/
├── core/                           # Platform-agnostic principles
│   ├── architecture/
│   │   ├── code-structure.md
│   │   ├── modular-architecture-principles.md
│   │   └── feature-extensibility.md
│   ├── development/
│   │   ├── asset-reusability.md
│   │   └── git-best-practices.md
│   ├── security/
│   │   └── security-principles.md          # NEW: Universal security principles
│   ├── testing/
│   │   └── testing-principles.md           # NEW: Universal testing principles
│   ├── deployment/
│   │   └── deployment-principles.md        # NEW: Universal CI/CD principles
│   ├── operations/
│   │   ├── monitoring-principles.md        # NEW: Universal observability principles
│   │   ├── budget-principles.md            # NEW: Universal cost optimization principles
│   │   └── platform-simplification-principles.md  # NEW: Universal simplification principles
│   └── README.md                           # Index of core principles
│
├── platforms/                      # Platform-specific implementations
│   └── firebase/
│       ├── firebase-best-practices.md
│       ├── firebase-deployment.md          # REFACTORED: Firebase CI/CD implementation
│       ├── firebase-monitoring.md          # REFACTORED: Firebase observability implementation
│       ├── firebase-security.md            # REFACTORED: Firebase security implementation
│       ├── firebase-testing.md             # REFACTORED: Firebase testing implementation
│       ├── firebase-finops.md              # RENAMED: finops-free-tier-maximization.md
│       ├── firebase-resilience.md          # REFACTORED: budget-resilience.md (Firebase parts)
│       └── README.md                       # Firebase implementation guide
│
├── meta/                           # Library maintenance
│   └── prompt-maintenance.md
│
└── README.md                       # Master index and usage guide
```

**Benefits**:
- Clear visual separation of concerns
- Easy to navigate and discover content
- Scalable for adding new platforms (e.g., `platforms/vercel/`, `platforms/aws/`)
- READMEs in each directory provide context and navigation

---

### Option 2: Naming Convention-Based Organization

```
.prompts/
├── [CORE] code-structure.md
├── [CORE] modular-architecture-principles.md
├── [CORE] feature-extensibility.md
├── [CORE] asset-reusability.md
├── [CORE] git-best-practices.md
├── [CORE] security-principles.md
├── [CORE] testing-principles.md
├── [CORE] deployment-principles.md
├── [CORE] monitoring-principles.md
├── [CORE] budget-principles.md
├── [CORE] platform-simplification-principles.md
├── [FIREBASE] firebase-best-practices.md
├── [FIREBASE] firebase-deployment.md
├── [FIREBASE] firebase-monitoring.md
├── [FIREBASE] firebase-security.md
├── [FIREBASE] firebase-testing.md
├── [FIREBASE] firebase-finops.md
├── [FIREBASE] firebase-resilience.md
├── [META] prompt-maintenance.md
└── README.md
```

**Benefits**:
- Simpler file structure (flat hierarchy)
- Clear prefix-based categorization
- Easy to grep/search by prefix

**Drawbacks**:
- Less scalable for multiple platforms
- Harder to navigate visually
- No directory-level READMEs for context

---

## Recommended Approach: Option 1 (Folder-Based)

**Rationale**:
- **Future-proof**: Easy to add new platforms (Vercel, AWS, Netlify) without cluttering
- **Discoverability**: Directory structure provides intuitive navigation
- **Contextual documentation**: Each directory can have its own README with specific guidance
- **Scalability**: Supports growth of prompt library without organizational debt
- **Separation of concerns**: Mirrors the architectural principles we're documenting

---

## Migration Strategy

### Phase 1: Create New Structure (No Deletions)
**Goal**: Build new organization without breaking existing references

1. **Create directory structure**:
   ```bash
   mkdir -p .prompts/core/{architecture,development,security,testing,deployment,operations}
   mkdir -p .prompts/platforms/firebase
   mkdir -p .prompts/meta
   ```

2. **Copy platform-agnostic files** (unchanged):
   ```bash
   # Architecture
   cp .prompts/code-structure.md .prompts/core/architecture/
   cp .prompts/modular-architecture-principles.md .prompts/core/architecture/
   cp .prompts/feature-extensibility.md .prompts/core/architecture/

   # Development
   cp .prompts/asset-reusability.md .prompts/core/development/
   cp .prompts/git-best-practices.md .prompts/core/development/

   # Meta
   cp .prompts/prompt-maintenance.md .prompts/meta/
   ```

3. **Copy Firebase-specific files** (unchanged):
   ```bash
   cp .prompts/firebase-best-practices.md .prompts/platforms/firebase/
   ```

### Phase 2: Refactor Mixed Files
**Goal**: Extract universal principles, create platform-specific implementations

For each mixed file:

1. **security-architecture.md** → Split into:
   - `.prompts/core/security/security-principles.md` (universal)
   - `.prompts/platforms/firebase/firebase-security.md` (Firebase-specific)

2. **testing-qa.md** → Split into:
   - `.prompts/core/testing/testing-principles.md` (universal)
   - `.prompts/platforms/firebase/firebase-testing.md` (Firebase-specific)

3. **deployment-cicd.md** → Split into:
   - `.prompts/core/deployment/deployment-principles.md` (universal CI/CD)
   - `.prompts/platforms/firebase/firebase-deployment.md` (Firebase implementation)

4. **monitoring-observability.md** → Split into:
   - `.prompts/core/operations/monitoring-principles.md` (universal observability)
   - `.prompts/platforms/firebase/firebase-monitoring.md` (Firebase implementation)

5. **platform-simplification.md** → Split into:
   - `.prompts/core/operations/platform-simplification-principles.md` (universal principle)
   - `.prompts/platforms/firebase/firebase-platform-guide.md` (Firebase-specific strategy)

6. **budget-resilience.md** → Split into:
   - `.prompts/core/operations/budget-principles.md` (universal cost optimization)
   - `.prompts/platforms/firebase/firebase-resilience.md` (Firebase resilience patterns)

7. **finops-free-tier-maximization.md** → Rename:
   - `.prompts/platforms/firebase/firebase-finops.md` (Firebase-specific)

### Phase 3: Create Navigation READMEs

1. **`.prompts/README.md`** (Master index):
   ```markdown
   # Development Prompts Library

   ## Core Principles (Platform-Agnostic)
   Universal software development principles applicable to any technology stack.
   - [Architecture](./core/architecture/)
   - [Development Practices](./core/development/)
   - [Security](./core/security/)
   - [Testing & QA](./core/testing/)
   - [Deployment & CI/CD](./core/deployment/)
   - [Operations](./core/operations/)

   ## Platform Implementations
   Platform-specific implementations of core principles.
   - [Firebase](./platforms/firebase/)

   ## Meta-Guidance
   - [Prompt Maintenance](./meta/prompt-maintenance.md)
   ```

2. **`.prompts/core/README.md`**:
   ```markdown
   # Core Principles

   Platform-agnostic software development principles.

   ## Architecture
   - [Code Structure](./architecture/code-structure.md)
   - [Modular Architecture](./architecture/modular-architecture-principles.md)
   - [Feature Extensibility](./architecture/feature-extensibility.md)

   [... continue for all categories ...]
   ```

3. **`.prompts/platforms/firebase/README.md`**:
   ```markdown
   # Firebase Implementation Guide

   Firebase-specific implementations of core principles.

   ## Quick Reference
   - [Best Practices](./firebase-best-practices.md)
   - [Deployment & CI/CD](./firebase-deployment.md)
   - [Security](./firebase-security.md)
   - [Testing](./firebase-testing.md)
   - [Monitoring](./firebase-monitoring.md)
   - [FinOps & Free Tier](./firebase-finops.md)
   - [Resilience](./firebase-resilience.md)
   ```

### Phase 4: Update CLAUDE.md References

Update `.prompts/` file references in `CLAUDE.md` to use new paths:

```markdown
**For Architecture Decisions:**
- `core/architecture/code-structure.md`
- `core/architecture/modular-architecture-principles.md`
- `core/architecture/feature-extensibility.md`
- `core/development/asset-reusability.md`

**For Firebase Implementation:**
- `platforms/firebase/firebase-best-practices.md`
- `platforms/firebase/firebase-deployment.md`
- `platforms/firebase/firebase-security.md`

**For Operations:**
- `core/deployment/deployment-principles.md`
- `platforms/firebase/firebase-deployment.md` (Firebase-specific)
- `core/operations/monitoring-principles.md`
- `platforms/firebase/firebase-monitoring.md` (Firebase-specific)
- `core/operations/budget-principles.md`
- `platforms/firebase/firebase-finops.md` (Firebase-specific)
```

### Phase 5: Deprecate Old Files (Final Step)

Once new structure is validated:

1. **Move old files to archive**:
   ```bash
   mkdir .prompts/.archive
   mv .prompts/*.md .prompts/.archive/
   ```

2. **Add deprecation notice** in `.prompts/.archive/README.md`:
   ```markdown
   # Archived Prompts

   These files have been reorganized into the new structure.
   See `.prompts/README.md` for the current organization.
   ```

---

## Template for Splitting Mixed Files

### Universal Principles File Template
```markdown
# [Topic] Principles

**Status**: Platform-Agnostic
**Last Updated**: YYYY-MM-DD

## Overview

Universal principles for [topic] that apply to any technology stack.

## Core Principles

### 1. [Principle Name]

**Definition**: [What is this principle]

**Why It Matters**: [Business/technical value]

**Implementation Patterns**: [Generic patterns, no platform specifics]

**Anti-Patterns**: [What to avoid, platform-agnostic]

---

## Platform Implementations

For platform-specific implementations of these principles, see:
- [Firebase](../../platforms/firebase/firebase-[topic].md)
- [Add other platforms as needed]
```

### Platform Implementation File Template
```markdown
# Firebase [Topic] Implementation

**Status**: Firebase-Specific
**Last Updated**: YYYY-MM-DD
**Core Principles**: See [../../../core/[category]/[topic]-principles.md]

## Overview

Firebase-specific implementation of [topic] principles using Firebase services.

## Firebase Services Used

- Firebase Service 1 (purpose)
- Firebase Service 2 (purpose)

## Implementation Patterns

### 1. [Pattern Name]

**Core Principle**: [Link to core principle]

**Firebase Implementation**:
```typescript
// Firebase-specific code
```

**Firebase-Specific Considerations**:
- Free tier limits
- Service quotas
- Firebase API patterns

---

## Cross-References

**Universal Principles**: [../../../core/[category]/[topic]-principles.md]
**Related Firebase Guides**: [other Firebase guides]
```

---

## Success Metrics

### Organizational Clarity
- [ ] All prompts categorized as Core, Platform-Specific, or Meta
- [ ] Clear navigation structure with READMEs
- [ ] No duplicated content between universal and platform-specific files

### Reusability
- [ ] Core principles can be referenced for any platform
- [ ] Platform-specific implementations clearly link to core principles
- [ ] New platforms can be added without modifying core principles

### Maintainability
- [ ] Each file has single responsibility (universal OR platform-specific)
- [ ] Cross-references maintained between core and platform files
- [ ] CLAUDE.md updated with new structure

### Developer Experience
- [ ] Easy to discover relevant prompts
- [ ] Clear path from principle to implementation
- [ ] Intuitive navigation (5 seconds to find relevant prompt)

---

## Timeline Estimate

**Phase 1: Create Structure** - 30 minutes
- Create directories
- Copy unchanged files
- Create basic READMEs

**Phase 2: Refactor Mixed Files** - 4-6 hours
- Split 7 mixed files into universal + Firebase-specific
- Ensure no content loss
- Maintain cross-references

**Phase 3: Navigation READMEs** - 1 hour
- Write comprehensive README files
- Create navigation guides
- Add examples

**Phase 4: Update CLAUDE.md** - 30 minutes
- Update file references
- Update decision framework
- Validate references

**Phase 5: Deprecation** - 15 minutes
- Archive old files
- Add deprecation notices

**Total Estimated Time**: 6-8 hours

---

## Risk Mitigation

### Risk: Breaking existing references in CLAUDE.md
**Mitigation**:
- Phase 1 creates new structure without deleting old files
- Phase 4 updates CLAUDE.md before deprecation
- Phase 5 only archives after validation

### Risk: Content duplication during split
**Mitigation**:
- Use clear templates for universal vs platform-specific
- Cross-reference between files
- Review each split file for duplication

### Risk: Loss of context during reorganization
**Mitigation**:
- Comprehensive READMEs in each directory
- Master index in `.prompts/README.md`
- Cross-references maintained in all files

### Risk: Incomplete separation of concerns
**Mitigation**:
- Clear criteria for universal vs platform-specific
- Review each file for Firebase-specific content
- Template ensures consistent separation

---

## Future Extensibility

This structure supports adding new platforms:

```
.prompts/platforms/
├── firebase/          # Existing
├── vercel/            # Future: Vercel implementation
│   ├── vercel-deployment.md
│   ├── vercel-monitoring.md
│   └── README.md
├── aws/               # Future: AWS implementation
│   ├── aws-deployment.md
│   ├── aws-monitoring.md
│   └── README.md
└── netlify/           # Future: Netlify implementation
    ├── netlify-deployment.md
    └── README.md
```

All platforms reference the same `core/` principles, ensuring consistency.

---

## Recommendation

**Proceed with Option 1 (Folder-Based Organization)** using the phased migration strategy.

This approach:
- ✅ Provides clear separation of universal vs platform-specific content
- ✅ Scales for future platforms
- ✅ Improves discoverability and navigation
- ✅ Maintains backward compatibility during migration
- ✅ Reduces risk through incremental changes

**Next Steps**:
1. Review and approve this plan
2. Begin Phase 1: Create new directory structure
3. Execute phases 2-5 incrementally
4. Validate with test usage of new structure
