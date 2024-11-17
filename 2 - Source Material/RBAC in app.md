---

## Role-Based Access Control (RBAC) Implementation

### Backend Implementation

---
## Role-Based Access Control (RBAC) Implementation

### Backend Implementation
### ASP.NET Core (MVC)

> [!notes] **Define Roles and Permissions**
```csharp
public class Roles
{
    public const string Admin = "Admin";
    public const string Moderator = "Moderator";
    public const string User = "User";
}

public class Permissions
{
    public const string ViewComments = "view:comments";
    public const string CreateComments = "create:comments";
    public const string UpdateComments = "update:comments";
    public const string DeleteComments = "delete:comments";
}
```

> [!notes] **Authorize Roles in Controllers**
```csharp
[Authorize(Roles = Roles.Admin)]
public IActionResult DeleteComment(int id)
{
    // Delete logic here
    return Ok();
}
```

> [!warning] **Dynamic Role-Permission Check**
```csharp
public bool HasPermission(ClaimsPrincipal user, string permission)
{
    var roles = user.FindFirst(ClaimTypes.Role)?.Value.Split(",");
    if (roles == null) return false;

    var rolePermissions = new Dictionary<string, List<string>>()
    {
        { Roles.Admin, new List<string> { Permissions.ViewComments, Permissions.CreateComments, Permissions.UpdateComments, Permissions.DeleteComments } },
        { Roles.Moderator, new List<string> { Permissions.ViewComments, Permissions.CreateComments, Permissions.DeleteComments } },
        { Roles.User, new List<string> { Permissions.ViewComments, Permissions.CreateComments } }
    };

    return roles.Any(role => rolePermissions.ContainsKey(role) && rolePermissions[role].Contains(permission));
}
```

---

### Spring Boot (MVC)

> [!notes] **Security Configuration**
```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
            .antMatchers("/comments/view").hasAnyRole("USER", "MODERATOR", "ADMIN")
            .antMatchers("/comments/create").hasAnyRole("USER", "MODERATOR", "ADMIN")
            .antMatchers("/comments/delete").hasRole("ADMIN")
            .and()
            .formLogin();
    }
}
```

> [!notes] **Permission Service**
```java
@Service
public class PermissionService {
    public boolean hasPermission(String role, String permission) {
        Map<String, List<String>> rolePermissions = Map.of(
            "ADMIN", List.of("view:comments", "create:comments", "update:comments", "delete:comments"),
            "MODERATOR", List.of("view:comments", "create:comments", "delete:comments"),
            "USER", List.of("view:comments", "create:comments")
        );

        return rolePermissions.getOrDefault(role, List.of()).contains(permission);
    }
}
```

---

### Frontend Implementation (React)

> [!notes] **Define Role-Permission Mapping**
```javascript
const ROLES = {
    admin: ["view:comments", "create:comments", "update:comments", "delete:comments"],
    moderator: ["view:comments", "create:comments", "delete:comments"],
    user: ["view:comments", "create:comments"],
};

function hasPermission(role, permission) {
    return ROLES[role]?.includes(permission);
}
```

> [!notes] **Conditional Rendering**
```jsx
const user = { id: "1", role: "user" };

return (
    <div>
        <h3>Comment</h3>
        {hasPermission(user.role, "delete:comments") && (
            <button>Delete Comment</button>
        )}
    </div>
);
```

---

### Go Implementation

> [!notes] **RBAC Middleware**
```go
package main

import "net/http"

var roles = map[string][]string{
    "admin":    {"view:comments", "create:comments", "update:comments", "delete:comments"},
    "moderator": {"view:comments", "create:comments", "delete:comments"},
    "user":      {"view:comments", "create:comments"},
}

func hasPermission(role, permission string) bool {
    permissions, exists := roles[role]
    if !exists {
        return false
    }
    for _, p := range permissions {
        if p == permission {
            return true
        }
    }
    return false
}

func RBACMiddleware(next http.Handler, role, permission string) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        if !hasPermission(role, permission) {
            http.Error(w, "Forbidden", http.StatusForbidden)
            return
        }
        next.ServeHTTP(w, r)
    })
}
```

> [!notes] **Route Protection**
```go
http.Handle("/comments/delete", RBACMiddleware(http.HandlerFunc(deleteComment), "admin", "delete:comments"))
```

---

### Best Practices

> [!notes] **RBAC Best Practices**
1. **Minimize Overlapping Roles**: Define clear and distinct roles.
2. **Use Claims-Based Authorization**: Use JWT tokens to store user roles and permissions.
3. **Avoid Hardcoding**: Store roles and permissions in a database or config file.
4. **Implement Middleware**: Centralize permission checks to maintain consistency.
5. **Enable Audit Logging**: Log role/permission changes and user actions for security.
