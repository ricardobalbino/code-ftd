# ============================================================
# D365 Solution Transport Task - Avanade Method v4.29.0
# ============================================================
# Owner: Roberto SM / Tiago Dev
# Atomic task for solution promotion between environments

---
id: d365-solution-transport
name: "D365 Solution Transport"
owner: roberto-sm
co-owner: tiago-dev

## Purpose

Guia passo-a-passo para exportar, empacotar e importar solutions D365
entre environments com validação em cada etapa.

## Pre-Conditions

- [ ] Todas stories do sprint estão Done
- [ ] Solution Checker executado (0 critical/high)
- [ ] Unit tests passando
- [ ] Code review aprovado (Carla QA)

## Steps

### Step 1: Export from Dev

```bash
# 1. Set dev environment
pac auth create --url https://ftd-dev.crm2.dynamics.com
pac auth select --index [n]

# 2. Solution Checker
pac solution check --path ./solutions/FTDCore.zip

# 3. Export unmanaged (for version control)
pac solution export --path ./solutions/FTDCore_unmanaged.zip --name FTDCore --managed false

# 4. Unpack for source control
pac solution unpack --zipfile ./solutions/FTDCore_unmanaged.zip --folder ./src/solutions/FTDCore --processCanvasApps
```

### Step 2: Build Managed

```bash
# Pack as managed
pac solution pack --zipfile ./solutions/FTDCore_managed.zip --folder ./src/solutions/FTDCore --type Managed
```

### Step 3: Import to Test

```bash
# 1. Set test environment
pac auth create --url https://ftd-test.crm2.dynamics.com
pac auth select --index [n]

# 2. Import managed
pac solution import --path ./solutions/FTDCore_managed.zip --activate-plugins --force-overwrite

# 3. Verify import
pac solution list
```

### Step 4: Validate in Test

- [ ] Solution imported without errors
- [ ] All plugins activated and registered
- [ ] Power Automate flows active (turn on if needed)
- [ ] Connection references configured
- [ ] Data reference migrated (if applicable)
- [ ] Smoke test passed (key scenarios work)

### Step 5: Promote to UAT (repeat Steps 3-4)

### Step 6: Promote to Prod (repeat Steps 3-4 with extra caution)

## Rollback Plan

```bash
# If import fails or causes issues:
# 1. Delete the imported managed solution
pac solution delete --solution-name FTDCore

# 2. Reimport previous version
pac solution import --path ./solutions/FTDCore_managed_v[previous].zip

# 3. Verify rollback
pac solution list
```

## Post-Transport Checklist

- [ ] All environments have same solution version
- [ ] Audit log checked for errors
- [ ] Users notified of changes
- [ ] Sprint status updated (Roberto SM)
