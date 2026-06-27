# Faribank Android 🏦📱

> **Advanced Programming Project 4 – KNTU**  
> Native Android Neobank Application | Java + XML | MVVM-Inspired Architecture

---

## 📋 Overview

**Faribank Android** is the GUI evolution of the Faribank neobank system, developed for the *Advanced Programming* course at K.N. Toosi University. This implementation decouples core business logic from previous CLI versions and delivers a native Android experience using **Java**, **XML layouts**, and **Android SDK**.

**Design Principle**: Separation of concerns — domain models remain platform-agnostic; UI layer handles presentation only.

---

## 🗂 Project Structure (Actual)

```
Project4_AP_KNTU_Android/
├── app/
│   └── src/main/
│       ├── java/ir/ac/kntu/project4/
│       │   ├── activities/          # UI controllers (Activities)
│       │   ├── util/                # Helpers: Calendar, Validators, Extensions
│       │   └── *.java               # Domain models:
│       │       ├── User.java, Person.java, Role.java
│       │       ├── Account.java, Bank.java
│       │       ├── Transaction.java, TransferPol.java
│       │       ├── Contact.java, SupportRequest.java
│       │       ├── Fund models: SavingsCapitalFunds.java, 
│       │       │   ResidualCapitalFunds.java, BonusCapitalFunds.java
│       │       └── Loan models: Loan.java, Installment.java, 
│       │           LoanCondition.java
│       │
│       ├── res/                     # Layouts, strings, drawables
│       └── AndroidManifest.xml
│
│   ├── src/androidTest/             # Instrumented UI tests
│   └── src/test/                    # Local unit tests
│
├── libs/                            # External JARs
│   ├── jfreechart-1.0.19.jar        # Bonus: financial charts
│   ├── junit-4.11.jar, hamcrest-core-1.3.jar
│   └── ...
│
├── config/checkstyle.xml            # Code style enforcement
├── build.gradle.kts                 # Module-level build config
├── settings.gradle.kts
├── gradle.properties
└── gradlew[.bat]                    # Gradle wrapper
```

> 📌 **Note**: Domain models reside in the root package (`ir.ac.kntu.project4`) for backward compatibility with Project3 logic. Refactoring into sub-packages (`model/`, `service/`) is recommended for future maintenance.

---

## ✨ Implemented Features

### 🔐 Authentication
- **Login**: Phone + password validation; error handling via `AlertDialog`
- **Sign Up**: Full registration with strong-password regex check + duplicate phone/national-ID prevention

### 🏠 Dashboard
- Balance display (`TextView`)
- Recent transactions (`RecyclerView`)
- Quick-action buttons: Deposit, Transfer, Funds, Loans, Contacts

### 👥 Contacts Management
- Add/Edit/Delete via `EditText` + `Button`
- Mutual-contact enforcement for transfers
- Searchable `RecyclerView` list

### 💸 Money Transfer
- Destination: manual input / recent accounts / mutual contacts
- Method selection: Card-to-Card, Pol, Paya, Fari-to-Fari (dynamic fee/limit display)
- Confirmation dialog + receipt generation
- Error handling: insufficient balance, invalid destination

### 💰 Investment Funds
| Fund | Implementation |
|------|---------------|
| **Savings** | Simple deposit/withdrawal UI |
| **Residual** | Auto-save toggle; remainder calc: `10^k - (digits × 0.75)` |
| **Bonus** | Lock-in period UI; maturity date display; profit credited by background thread |

### 🎯 Loan Management
- **Request**: Form with amount/duration/type; status tracking (`Pending`/`Approved`/`Rejected`)
- **Installments**: List view with payment button; receipt generation on success

### 🎫 Support Requests
- Submit ticket per module
- View history with status + admin response
- Auto-reply simulation via background thread

### 📊 Bonus: Financial Charts
- Implemented with **JFreeChart** (`libs/jfreechart-1.0.19.jar`)
- Balance trend line charts + transaction category pie charts
- Export to PNG via `OrsonPDF`/`FreeSVG` support

---

## ⚙️ Technical Details

| Aspect | Implementation |
|--------|---------------|
| **Language** | Java 8 (Android-compatible) |
| **Min SDK** | 24 (Android 7.0) |
| **UI Framework** | XML layouts + Material Components |
| **Lists** | `RecyclerView` with custom `Adapter` + `ViewHolder` |
| **Navigation** | `Intent`-based activity transitions |
| **Persistence** | Serialized Java objects (core); `Room` ready (bonus) |
| **Concurrency** | `AsyncTask`/`Executor` for background ops; `runOnUiThread` for UI updates |
| **Validation** | Reusable validators in `util/`; regex for password/phone |
| **Time Simulation** | `util.Date.java` wrapper around `ir.ac.kntu.util.Calendar` (mandatory for testing) |

### Background Automation (No UI)
```java
// Simplified: Admin automation via ScheduledExecutor
public class AutomationManager {
    private final ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(4);
    
    public void start() {
        scheduler.scheduleAtFixedRate(this::processPayaTransfers, 0, 1, HOURS);
        scheduler.scheduleAtFixedRate(this::distributeFundProfits, 0, 30, DAYS);
        scheduler.scheduleAtFixedRate(this::autoReplySupportRequests, 0, 5, MINUTES);
        scheduler.scheduleAtFixedRate(this::evaluateLoanRequests, 0, 10, MINUTES);
    }
}
```

---

## 🚀 Build & Run

```bash
# 1. Clone
git clone https://github.com/mahajialirezaei/Faribank-android.git
cd Faribank-android

# 2. Build via Gradle Wrapper
./gradlew assembleDebug          # Linux/macOS
gradlew.bat assembleDebug       # Windows

# 3. Install & Run
./gradlew installDebug
# Or open in Android Studio → Run ▶️
```

### Requirements
| Component | Version |
|-----------|---------|
| Android Studio | Flamingo+ (2023.2.1) |
| JDK | 11 (embedded) |
| Android SDK | API 33 + Build Tools 33.0.2 |
| Gradle | 8.0+ (wrapper included) |

---

## 🧪 Testing

```bash
# Local unit tests (JVM)
./gradlew test

# Instrumented UI tests (device/emulator)
./gradlew connectedAndroidTest
```

- **Unit Tests**: `src/test/java/` — logic validation for funds, transfers, loans
- **UI Tests**: `src/androidTest/java/` — Espresso tests for critical flows (login, transfer)

---

## 🔧 Code Quality

- **Checkstyle**: Enforced via `config/checkstyle.xml`
  ```bash
  ./gradlew checkstyleMain
  ```
- **Conventional Commits**: `feat(loan): add installment payment UI`, `fix(transfer): validate Paya fee calculation`
- **Clean Code**: No business logic in Activities; models are pure POJOs; utilities are stateless

---

## 📚 Resources

- [Android Developers Guide](https://developer.android.com/guide)
- [RecyclerView Best Practices](https://developer.android.com/guide/topics/ui/layout/recyclerview)
- [JFreeChart User Guide](https://www.jfree.org/jfreechart/userguide/)
- [Effective Java (Bloch)](https://www.oreilly.com/library/view/effective-java-3rd/9780134686097/) — for OOP design decisions

---

## 🤝 Contributing

1. Fork → create feature branch: `git checkout -b feat/fund-chart-export`
2. Implement + write tests
3. Run `./gradlew check` to ensure style + test compliance
4. PR with:  
   - Screenshots for UI changes  
   - Test coverage report  
   - Brief rationale for design decisions

> ✅ **Pre-Merge Checklist**:  
> - [ ] No logic in Activities (delegate to util/model)  
> - [ ] Background ops off main thread  
> - [ ] All user inputs validated + localized error messages  
> - [ ] `RecyclerView` uses `DiffUtil` for efficient updates  
> - [ ] `Date.java` uses `ir.ac.kntu.util.Calendar` for time ops  

---

*Faribank Android — Clean architecture, scalable design, user-first experience.* 💳✨

---

🛠 **Developer**: [Mohammad Amin Haji Alirezaei](https://github.com/mahajialirezaei)  
🎓 **Course**: Advanced Programming – K.N. Toosi University of Technology  
📅 **Semester**: Spring/Summer 2024
