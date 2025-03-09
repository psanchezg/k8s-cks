# RBAC (Role-Based Access Control)

## Scenario 1
In this scenario, user "jane" should be able to get, list secrets in red namespace, and can only delete secrets in the blue namespace.

**Create Role**

```
k create role secrets-access --verb get,list --resource secrets -n red
k create role secrets-access --verb delete --resource secrets -n blue
```

**Create RoleBinding**

```
k create rolebinding secrets-access --role secrets-access --user jane -n red
k create rolebinding secrets-access --role secrets-access --user jane -n blue
```

**Check whether an action is allowed or denied**

```
k auth can-i get secrets --as jane -n red # Yes
k auth can-i list secrets --as jane -n red # Yes
k auth can-i delete secrets --as jane -n red # No
k auth can-i delete secrets --as jane -n blue # Yes
k auth can-i get secrets --as jane -n blue # No
```

## Scenario 2

In this scenario, service account "automation" should be able to get, list secrets in red namespace, and can only delete secrets in the blue namespace.

**Create service account**

```
k create sa automation -n red
k create sa automation -n blue
```

**Create Role**

```
k create role secrets-access-sa --verb get,list --resource secrets -n red
k create role secrets-modify --verb delete --resource secrets -n blue
```

**Create RoleBinding**

```
k create rolebinding secrets-access-sa --role secrets-access-sa --serviceaccount red:automation -n red
k create rolebinding secrets-modify --role secrets-modify --serviceaccount blue:automation -n blue
```

**Check whether an action is allowed**

```
k auth can-i get secrets --as system:serviceaccount:red:automation -n red # Yes
k auth can-i delete secrets --as system:serviceaccount:red:automation -n red # No
k auth can-i delete secret --as system:serviceaccount:blue:automation -n blue # Yes
```