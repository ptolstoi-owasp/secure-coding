# 🛡️ OWASP Top 10:2025 – Praxisleitfaden für Entwickler

Dieses Repository bietet eine praxisnahe Übersicht der [OWASP Top 10 für 2025](https://owasp.org/Top10/2025/). Es zeigt nicht nur die theoretischen Schwachstellen, sondern liefert konkrete Code-Beispiele (Anti-Patterns) und deren sichere Lösungen, primär in **C# (ASP.NET Core)** und **TypeScript/JavaScript (Node.js/React)**.

---

## A01:2025 – Broken Access Control (Defekte Zugriffskontrolle)
**Beschreibung:** Nutzer können auf Daten oder Funktionen zugreifen, für die sie keine Berechtigung haben. Die bloße Tatsache, dass jemand eingeloggt ist, reicht nicht aus.

**Das Anti-Pattern (TypeScript / Express):**
```typescript
// UNSICHER: Keine Prüfung, ob die angefragte ID dem Nutzer gehört (IDOR-Schwachstelle).
app.get('/api/account/:accountId', async (req, res) => {
    const account = await db.getAccount(req.params.accountId);
    res.json(account); 
});
```

**Die sichere Lösung:**
```typescript
// SICHER: Daten-basierte Autorisierungsprüfung
app.get('/api/account/:accountId', async (req, res) => {
    const loggedInUserId = req.user.id; 
    const account = await db.getAccount(req.params.accountId);
    
    if (!account) return res.status(404).json({ error: 'Nicht gefunden.' });

    // Prüfung der Besitzverhältnisse
    if (account.ownerId !== loggedInUserId && !req.user.isAdmin) {
        return res.status(403).json({ error: 'Keine Berechtigung.' });
    }

    res.json(account); 
});
```

---

## A02:2025 – Security Misconfiguration (Sicherheitsfehlkonfiguration)
**Beschreibung:** Unsichere Standardeinstellungen, zu offene CORS-Richtlinien oder das Fehlen von HTTP Security Headern.

**Das Anti-Pattern (TypeScript / CORS):**
```typescript
// UNSICHER: Erlaubt jedem Frontend weltweit den Zugriff auf die API.
app.use(cors({ origin: '*' }));
```

**Die sichere Lösung (C# / ASP.NET Core):**
```csharp
// SICHER: Strikte CORS-Richtlinien und Security Header
builder.Services.AddCors(options => {
    options.AddPolicy("StrictCors", policy => {
        policy.WithOrigins("[https://meine-app.de](https://meine-app.de)")
              .AllowAnyHeader()
              .AllowAnyMethod()
              .AllowCredentials(); // Wichtig für HttpOnly-Cookies
    });
});

var app = builder.Build();
app.UseCors("StrictCors");

// Security Header Middleware
app.Use(async (context, next) => {
    context.Response.Headers.Append("Content-Security-Policy", "default-src 'self';");
    context.Response.Headers.Append("X-Frame-Options", "DENY");
    context.Response.Headers.Append("X-Content-Type-Options", "nosniff");
    await next();
});
```

---

## A03:2025 – Software Supply Chain Failures (Fehler in der Lieferkette)
**Beschreibung:** Die Nutzung von kompromittierten Fremdbibliotheken (npm, NuGet) oder unsicheren CI/CD-Pipelines.

**Die sichere Lösung (npm & NuGet):**
Vertraue niemals blinden Updates. Nutze Lockfiles, um exakte Versionen und deren kryptografische Hashes einzufrieren.

**Für Node.js (CI/CD Pipeline):**
```bash
# Nutze immer Clean Install auf dem Build-Server, um die package-lock.json zu erzwingen
RUN npm ci --ignore-scripts
```

**Für .NET (in der `.csproj`):**
```xml
<PropertyGroup>
  <RestorePackagesWithLockFile>true</RestorePackagesWithLockFile>
  <RestoreLockedMode>true</RestoreLockedMode>
</PropertyGroup>
```

---

## A04:2025 – Cryptographic Failures (Kryptografische Fehler)
**Beschreibung:** Mangelhafter Schutz sensibler Daten, z. B. durch veraltete Hashing-Algorithmen wie MD5 oder SHA-256 für Passwörter.

**Die sichere Lösung (C# / BCrypt):**
Nutze etablierte Key Derivation Functions wie Argon2id (OWASP Empfehlung) oder BCrypt.

```csharp
using BCrypt.Net;

public class SecurityService 
{
    public string HashPassword(string plainText) {
        // Generiert Hash inkl. Salt mit einem Work-Factor von 12
        return BCrypt.Net.BCrypt.EnhancedHashPassword(plainText, 12);
    }

    public bool Verify(string plainText, string hashFromDb) {
        return BCrypt.Net.BCrypt.EnhancedVerify(plainText, hashFromDb);
    }
}
```

---

## A05:2025 – Injection (Injektionsangriffe)
**Beschreibung:** Nutzereingaben werden als Code ausgeführt (z. B. SQL-Injection).

**Das Anti-Pattern:**
String-Verkettung bei Datenbankabfragen: `$"SELECT * FROM users WHERE name = '{input}'"`

**Die sichere Lösung (TypeScript / PostgreSQL):**
```typescript
// SICHER: Nutzung von parametrisierten Abfragen (Prepared Statements)
const username = req.body.username;
// $1 trennt die Daten strikt vom ausführbaren SQL-Code
const query = 'SELECT id, email FROM users WHERE username = $1'; 
const result = await db.query(query, [username]);
```

---

## A06:2025 – Insecure Design (Unsicheres Design)
**Beschreibung:** Architekturfehler, die nicht durch Code-Fixes, sondern nur durch Threat Modeling behoben werden können (z. B. fehlendes Rate-Limiting).

**Die sichere Lösung (C# / ASP.NET Core Rate Limiting):**
```csharp
// In Program.cs
builder.Services.AddRateLimiter(options => {
    options.AddFixedWindowLimiter("LoginLimit", opt => {
        opt.PermitLimit = 5; // Max 5 Versuche...
        opt.Window = TimeSpan.FromMinutes(1); // ...pro Minute
    });
    options.RejectionStatusCode = 429;
});

// Im Controller
[HttpPost("login")]
[EnableRateLimiting("LoginLimit")]
public IActionResult Login([FromBody] LoginDto dto) { /* ... */ }
```

---

## A07:2025 – Authentication Failures (Authentifizierungsfehler & CSRF)
**Beschreibung:** Unsichere Speicherung von JWT-Tokens im `localStorage` (XSS-Gefahr) und fehlender Schutz vor Cross-Site Request Forgery (CSRF).

**Die sichere Lösung (C# JWT in HttpOnly Cookies):**
```csharp
[HttpPost("login")]
public IActionResult Login([FromBody] LoginDto data)
{
    var token = _tokenService.GenerateJwt(user);

    // SICHER: Token vor JavaScript verstecken (HttpOnly)
    var cookieOptions = new CookieOptions {
        HttpOnly = true,   // XSS-Schutz
        Secure = true,     // HTTPS Zwang
        SameSite = SameSiteMode.Strict // CSRF-Basisschutz
    };

    Response.Cookies.Append("auth_token", token, cookieOptions);
    return Ok(new { message = "Login erfolgreich" });
}
```

---

## A08:2025 – Software and Data Integrity Failures (Integritätsfehler)
**Beschreibung:** Das System vertraut manipulierten Daten, z. B. durch unsichere Deserialisierung.

**Die sichere Lösung (C# / System.Text.Json):**
```csharp
// SICHER: Das Framework erzwingt den Typ 'UserDataDto'. 
// Angreifer können keine böswilligen Systemklassen injizieren.
public class UserDataDto { public string Name { get; set; } }

[HttpPost("process")]
public IActionResult Process([FromBody] string jsonPayload)
{
    try {
        var data = JsonSerializer.Deserialize<UserDataDto>(jsonPayload);
        return Ok(data);
    } catch (JsonException) {
        return BadRequest("Ungültiges Format.");
    }
}
```

---

## A09:2025 – Security Logging and Alerting Failures (Logging-Fehler)
**Beschreibung:** Angriffe bleiben unentdeckt, weil kritische Ereignisse nicht strukturiert protokolliert werden.

**Die sichere Lösung (C# / ASP.NET Core):**
```csharp
// SICHER: Strukturiertes JSON-Logging ohne sensible Daten (wie Passwörter)
var ip = HttpContext.Connection.RemoteIpAddress?.ToString();

if (!isValidLogin) {
    _logger.LogWarning(
        "Fehlgeschlagener Login für: {Username} von IP: {IpAddress}", 
        request.Username, ip
    );
    return Unauthorized();
}
```

---

## A10:2025 – Mishandling of Exceptional Conditions (Fehler bei Ausnahmezuständen)
**Beschreibung:** Das System scheitert "offen" (Fail Open) statt sicher (Fail Closed) und gewährt versehentlich Rechte bei Abstürzen.

**Die sichere Lösung (C# / Fail Closed):**
```csharp
public async Task<bool> IsUserAdminAsync(string userId)
{
    // SICHER: Standardmäßig auf 'false' setzen
    bool isAdmin = false; 

    try {
        isAdmin = await _db.CheckAdminStatusAsync(userId);
    } catch (Exception ex) {
        _logger.LogError(ex, "DB Fehler bei Admin-Prüfung.");
        // Im Fehlerfall bleibt isAdmin = false (Fail Closed)
    }

    return isAdmin;
}
