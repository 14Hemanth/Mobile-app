# MASTER ANDROID DEPLOYMENT FILE - FULL PROJECT CODE DUMP

This document contains the complete code snapshot for the Android app project from the android-app folder.

## Included Files

- .github\workflows\android-apk.yml
- .gitignore
- capacitor.config.ts
- index.html
- package.json
- README.md
- src\App.css
- src\App.jsx
- src\components\Dashboard\MoodChart.css
- src\components\Dashboard\MoodChart.jsx
- src\components\Dashboard\MoodDashboard.css
- src\components\Dashboard\MoodDashboard.jsx
- src\components\Expenses\ExpenseCharts.css
- src\components\Expenses\ExpenseCharts.jsx
- src\components\Expenses\ExpenseForm.css
- src\components\Expenses\ExpenseForm.jsx
- src\components\Expenses\ExpenseList.css
- src\components\Expenses\ExpenseList.jsx
- src\components\Journal\JournalCard.css
- src\components\Journal\JournalCard.jsx
- src\components\Journal\JournalForm.css
- src\components\Journal\JournalForm.jsx
- src\components\Journal\JournalList.css
- src\components\Journal\JournalList.jsx
- src\components\Layout\Footer.css
- src\components\Layout\Footer.jsx
- src\components\Layout\Header.css
- src\components\Layout\Header.jsx
- src\components\Learning\LearningCard.css
- src\components\Learning\LearningCard.jsx
- src\components\Learning\LearningForm.css
- src\components\Learning\LearningForm.jsx
- src\components\Learning\LearningList.css
- src\components\Learning\LearningList.jsx
- src\hooks\useExpenses.js
- src\hooks\useLearnings.js
- src\hooks\useLocalStorage.js
- src\main.jsx
- src\pages\Dashboard.css
- src\pages\Dashboard.jsx
- src\pages\Expenses.css
- src\pages\Expenses.jsx
- src\pages\Home.css
- src\pages\Home.jsx
- src\pages\Journals.css
- src\pages\Journals.jsx
- src\pages\Learning.css
- src\pages\Learning.jsx
- src\styles\global.css
- src\styles\variables.css
- src\utils\dateHelpers.js
- src\utils\indexedDb.js
- src\utils\moodCalculator.js
- tsconfig.json
- tsconfig.node.json
- vite.config.js

## Full Code

### FILE: .github\workflows\android-apk.yml

```text
name: Build Android APK

on:
  workflow_dispatch:

jobs:
  build-apk:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: "20"

      - name: Install dependencies
        run: npm install

      - name: Build web app
        run: npm run build

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: "temurin"
          java-version: "17"

      - name: Add Android platform
        run: npx cap add android

      - name: Sync Capacitor
        run: npm run android:sync

      - name: Build debug APK
        run: |
          cd android
          chmod +x ./gradlew
          ./gradlew assembleDebug

      - name: Upload APK artifact
        uses: actions/upload-artifact@v4
        with:
          name: daily-journal-debug-apk
          path: android/app/build/outputs/apk/debug/app-debug.apk
```

### FILE: .gitignore

```text
node_modules/
dist/
android/
.DS_Store
*.local
```

### FILE: capacitor.config.ts

```text
import type { CapacitorConfig } from "@capacitor/cli";

const config: CapacitorConfig = {
  appId: "com.dailyjournal.mobile",
  appName: "Daily Journal",
  webDir: "dist",
  bundledWebRuntime: false,
};

export default config;
```

### FILE: index.html

```text
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta
      name="description"
      content="Daily Journal - Your personal space for reflection and growth"
    />
    <meta name="theme-color" content="#6366f1" />
    <title>Daily Journal - Track Your Mood & Thoughts</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

### FILE: package.json

```text
{
  "name": "daily-journal-android",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "android:add": "npx cap add android",
    "android:sync": "npx cap sync android",
    "android:open": "npx cap open android",
    "android:apk:debug": "cd android && gradlew.bat assembleDebug",
    "android:apk:release": "cd android && gradlew.bat assembleRelease"
  },
  "dependencies": {
    "@capacitor/android": "^6.2.0",
    "@capacitor/core": "^6.2.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.20.0",
    "recharts": "^2.10.3"
  },
  "devDependencies": {
    "@capacitor/cli": "^6.2.0",
    "@types/react": "^18.2.43",
    "@types/react-dom": "^18.2.17",
    "@vitejs/plugin-react": "^4.2.1",
    "vite": "^5.0.8"
  }
}
```

### FILE: README.md

```text
# Daily Journal Android App Folder

This folder contains an isolated, Android-ready project for your Daily Journal app.

## Single-file deployment reference

Use this file as the main source of truth:

- `MASTER_ANDROID_DEPLOYMENT_FILE.md`

## Features included

- Daily journal entries with mood tracking
- Daily learnings tracker
- Daily expenses tracker with statistics
- Persistent local database via IndexedDB
- Android packaging via Capacitor

## Local build (Windows)

1. Install dependencies

```bash
npm install
```

2. Build web assets

```bash
npm run build
```

3. Add Android project (first time only)

```bash
npm run android:add
```

4. Sync web assets to Android

```bash
npm run android:sync
```

5. Build debug APK

```bash
npm run android:apk:debug
```

APK output:

```text
android/app/build/outputs/apk/debug/app-debug.apk
```

## Publish and get APK from GitHub

A GitHub Actions workflow is included in this folder:

- `.github/workflows/android-apk.yml`

When you push this folder as a repository to GitHub, run the workflow manually from Actions tab, then download the generated APK artifact.
```

### FILE: src\App.css

```text
/**
 * App Styles
 */

.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: var(--background);
  color: var(--text-primary);
}

.app-main {
  flex: 1;
  padding: var(--spacing-lg) 0;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-md);
}

@media (max-width: 768px) {
  .app-main {
    padding: var(--spacing-md) 0;
  }

  .container {
    padding: 0 var(--spacing-sm);
  }
}
```

### FILE: src\App.jsx

```text
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import { useJournals } from "./hooks/useLocalStorage";
import Header from "./components/Layout/Header";
import Footer from "./components/Layout/Footer";
import Home from "./pages/Home";
import Journals from "./pages/Journals";
import Dashboard from "./pages/Dashboard";
import Learning from "./pages/Learning";
import Expenses from "./pages/Expenses";
import JournalList from "./components/Journal/JournalList";
import "./styles/global.css";
import "./App.css";

/**
 * Main App Component
 * Manages routing and journal state
 */
function App() {
  const { journals, addJournal, deleteJournal } = useJournals();

  return (
    <Router>
      <div className="app">
        <Header />
        <main className="app-main">
          <Routes>
            <Route path="/" element={<Home />} />
            <Route
              path="/journals"
              element={
                <>
                  <div className="container">
                    <Journals journals={journals} onAddJournal={addJournal} />
                  </div>
                  <JournalList journals={journals} onDelete={deleteJournal} />
                </>
              }
            />
            <Route
              path="/dashboard"
              element={
                <Dashboard
                  journals={journals}
                  onDeleteJournal={deleteJournal}
                />
              }
            />
            <Route path="/learning" element={<Learning />} />
            <Route path="/expenses" element={<Expenses />} />
          </Routes>
        </main>
        <Footer />
      </div>
    </Router>
  );
}

export default App;
```

### FILE: src\components\Dashboard\MoodChart.css

```text
/**
 * MoodChart Styles
 */

.mood-chart-container {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  box-shadow: var(--shadow-md);
  animation: slideIn var(--transition-base);
}

.mood-chart-empty {
  text-align: center;
  padding: var(--spacing-2xl) var(--spacing-xl);
  color: var(--text-secondary);
}

.chart-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: var(--spacing-lg);
  margin-bottom: var(--spacing-2xl);
}

.stat-card {
  background: linear-gradient(
    135deg,
    rgba(99, 102, 241, 0.1),
    rgba(129, 140, 248, 0.05)
  );
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: var(--spacing-lg);
  text-align: center;
  transition: all var(--transition-base);
}

.stat-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
  border-color: var(--primary-color);
}

.stat-label {
  display: block;
  font-size: var(--font-size-sm);
  color: var(--text-secondary);
  margin-bottom: var(--spacing-sm);
  font-weight: 500;
}

.stat-value {
  display: block;
  font-size: 2rem;
  font-weight: 700;
  color: var(--primary-color);
}

.stat-out-of {
  display: block;
  font-size: var(--font-size-sm);
  color: var(--text-tertiary);
  margin-top: var(--spacing-xs);
}

.chart-section {
  margin-top: var(--spacing-2xl);
  padding-top: var(--spacing-xl);
  border-top: 1px solid var(--border);
}

.chart-section h3 {
  margin-bottom: var(--spacing-lg);
  color: var(--text-primary);
}

.chart-tooltip {
  background-color: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  padding: var(--spacing-md);
  box-shadow: var(--shadow-lg);
  z-index: 1000;
}

.tooltip-week {
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: var(--spacing-xs);
}

.tooltip-mood,
.tooltip-entries {
  font-size: var(--font-size-sm);
  color: var(--text-secondary);
  margin: var(--spacing-xs) 0;
}

.tooltip-mood strong,
.tooltip-entries strong {
  color: var(--primary-color);
  font-weight: 600;
}

@media (max-width: 768px) {
  .mood-chart-container {
    padding: var(--spacing-lg);
  }

  .chart-stats {
    grid-template-columns: repeat(2, 1fr);
    gap: var(--spacing-md);
  }

  .stat-value {
    font-size: 1.5rem;
  }

  .chart-section h3 {
    font-size: var(--font-size-lg);
  }
}
```

### FILE: src\components\Dashboard\MoodChart.jsx

```text
import {
  LineChart,
  Line,
  BarChart,
  Bar,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  Legend,
  ResponsiveContainer,
} from "recharts";
import {
  getWeeklyMoodData,
  calculateMoodStats,
} from "../../utils/moodCalculator";
import "./MoodChart.css";

/**
 * MoodChart Component
 * Displays mood trends using Recharts
 */
const MoodChart = ({ journals }) => {
  const chartData = getWeeklyMoodData(journals);
  const stats = calculateMoodStats(journals);

  const CustomTooltip = ({ active, payload }) => {
    if (active && payload && payload[0]) {
      const data = payload[0].payload;
      return (
        <div className="chart-tooltip">
          <p className="tooltip-week">{data.week}</p>
          <p className="tooltip-mood">
            Average Mood: <strong>{data.mood.toFixed(1)}</strong>
          </p>
          <p className="tooltip-entries">
            Entries: <strong>{data.entries}</strong>
          </p>
        </div>
      );
    }
    return null;
  };

  if (chartData.length === 0) {
    return (
      <div className="mood-chart-empty">
        <p>
          No data available. Start adding journal entries to see your mood
          trends!
        </p>
      </div>
    );
  }

  return (
    <div className="mood-chart-container">
      <div className="chart-stats">
        <div className="stat-card">
          <span className="stat-label">Average Mood</span>
          <span className="stat-value">{stats.average.toFixed(1)}</span>
          <span className="stat-out-of">/5.0</span>
        </div>
        <div className="stat-card">
          <span className="stat-label">Highest</span>
          <span className="stat-value">{stats.highest}</span>
        </div>
        <div className="stat-card">
          <span className="stat-label">Lowest</span>
          <span className="stat-value">{stats.lowest}</span>
        </div>
        <div className="stat-card">
          <span className="stat-label">Total Entries</span>
          <span className="stat-value">{stats.total}</span>
        </div>
      </div>

      <div className="chart-section">
        <h3>ðŸ“ˆ Weekly Mood Trend</h3>
        <ResponsiveContainer width="100%" height={300}>
          <LineChart data={chartData}>
            <CartesianGrid strokeDasharray="3 3" stroke="var(--border)" />
            <XAxis
              dataKey="week"
              tick={{ fontSize: 12, fill: "var(--text-secondary)" }}
            />
            <YAxis
              domain={[0, 5]}
              tick={{ fontSize: 12, fill: "var(--text-secondary)" }}
            />
            <Tooltip content={<CustomTooltip />} />
            <Legend wrapperStyle={{ paddingTop: "20px" }} />
            <Line
              type="monotone"
              dataKey="mood"
              stroke="var(--primary-color)"
              strokeWidth={3}
              dot={{ fill: "var(--primary-color)", r: 6 }}
              activeDot={{ r: 8 }}
              name="Mood Score"
            />
          </LineChart>
        </ResponsiveContainer>
      </div>

      <div className="chart-section">
        <h3>ðŸ“Š Weekly Mood Distribution</h3>
        <ResponsiveContainer width="100%" height={300}>
          <BarChart data={chartData}>
            <CartesianGrid strokeDasharray="3 3" stroke="var(--border)" />
            <XAxis
              dataKey="week"
              tick={{ fontSize: 12, fill: "var(--text-secondary)" }}
            />
            <YAxis
              domain={[0, 5]}
              tick={{ fontSize: 12, fill: "var(--text-secondary)" }}
            />
            <Tooltip content={<CustomTooltip />} />
            <Legend wrapperStyle={{ paddingTop: "20px" }} />
            <Bar
              dataKey="mood"
              fill="var(--primary-light)"
              name="Mood Score"
              radius={[8, 8, 0, 0]}
            />
          </BarChart>
        </ResponsiveContainer>
      </div>
    </div>
  );
};

export default MoodChart;
```

### FILE: src\components\Dashboard\MoodDashboard.css

```text
/**
 * MoodDashboard Styles
 */

.mood-dashboard {
  max-width: 1200px;
  margin: 0 auto;
  padding: var(--spacing-lg);
}

.dashboard-header {
  text-align: center;
  margin-bottom: var(--spacing-2xl);
}

.dashboard-header h1 {
  margin-bottom: var(--spacing-sm);
}

.dashboard-header p {
  color: var(--text-secondary);
  font-size: var(--font-size-lg);
}

.dashboard-empty {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-2xl);
  text-align: center;
  color: var(--text-secondary);
}

.dashboard-empty h2 {
  margin-bottom: var(--spacing-md);
}

/* Mood Distribution */
.mood-distribution {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  margin-bottom: var(--spacing-xl);
  box-shadow: var(--shadow-md);
  animation: slideIn var(--transition-base);
}

.mood-distribution h2 {
  margin-bottom: var(--spacing-lg);
}

.distribution-grid {
  display: grid;
  gap: var(--spacing-lg);
}

.distribution-item {
  display: grid;
  grid-template-columns: 60px 1fr 80px;
  gap: var(--spacing-lg);
  align-items: center;
}

.distribution-emoji {
  font-size: 2rem;
  text-align: center;
}

.distribution-bar {
  height: 30px;
  background-color: var(--surface-alt);
  border-radius: var(--radius-md);
  overflow: hidden;
  border: 1px solid var(--border);
}

.distribution-fill {
  height: 100%;
  transition: width var(--transition-base);
  min-width: 2px;
}

.distribution-count {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
}

.count-number {
  display: block;
  font-weight: 600;
  color: var(--text-primary);
  font-size: var(--font-size-lg);
}

.count-percent {
  display: block;
  font-size: var(--font-size-sm);
  color: var(--text-secondary);
}

/* Insights */
.dashboard-insights {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  box-shadow: var(--shadow-md);
  animation: slideIn var(--transition-base);
  animation-delay: 100ms;
}

.dashboard-insights h2 {
  margin-bottom: var(--spacing-lg);
}

.insights-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--spacing-lg);
}

.insight-card {
  background: linear-gradient(
    135deg,
    rgba(99, 102, 241, 0.05),
    rgba(129, 140, 248, 0.1)
  );
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: var(--spacing-lg);
  transition: all var(--transition-base);
}

.insight-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
  border-color: var(--primary-color);
}

.insight-card h3 {
  color: var(--primary-color);
  margin-bottom: var(--spacing-md);
}

.insight-card p {
  color: var(--text-secondary);
  font-size: var(--font-size-sm);
  line-height: var(--line-height-relaxed);
  margin: 0;
}

.insight-card strong {
  color: var(--text-primary);
  font-weight: 600;
}

@media (max-width: 768px) {
  .mood-dashboard {
    padding: var(--spacing-md);
  }

  .dashboard-header h1 {
    font-size: var(--font-size-2xl);
  }

  .dashboard-header p {
    font-size: var(--font-size-base);
  }

  .distribution-item {
    grid-template-columns: 50px 1fr 70px;
    gap: var(--spacing-md);
  }

  .distribution-emoji {
    font-size: 1.5rem;
  }

  .insights-grid {
    grid-template-columns: 1fr;
  }
}
```

### FILE: src\components\Dashboard\MoodDashboard.jsx

```text
import MoodChart from "./MoodChart";
import {
  calculateMoodStats,
  getMoodEmoji,
  MOOD_MAP,
} from "../../utils/moodCalculator";
import "./MoodDashboard.css";

/**
 * MoodDashboard Component
 * Main dashboard with mood analytics and insights
 */
const MoodDashboard = ({ journals }) => {
  const stats = calculateMoodStats(journals);

  // Calculate mood distribution
  const moodDistribution = {};
  Object.keys(MOOD_MAP).forEach((mood) => {
    moodDistribution[mood] = 0;
  });

  journals.forEach((journal) => {
    moodDistribution[journal.mood]++;
  });

  return (
    <div className="mood-dashboard">
      <div className="dashboard-header">
        <h1>ðŸ“Š Your Mood Analytics</h1>
        <p>Track your emotional well-being over time</p>
      </div>

      {journals.length === 0 ? (
        <div className="dashboard-empty">
          <h2>No data yet</h2>
          <p>
            Start by creating your first journal entry to begin tracking your
            mood!
          </p>
        </div>
      ) : (
        <>
          {/* Mood Distribution */}
          <div className="mood-distribution">
            <h2>ðŸ’­ Mood Distribution</h2>
            <div className="distribution-grid">
              {Object.entries(MOOD_MAP).map(([value, emoji]) => {
                const count = moodDistribution[value];
                const percentage =
                  journals.length > 0 ? (count / journals.length) * 100 : 0;

                return (
                  <div key={value} className="distribution-item">
                    <div className="distribution-emoji">{emoji}</div>
                    <div className="distribution-bar">
                      <div
                        className="distribution-fill"
                        style={{
                          width: `${percentage}%`,
                          backgroundColor: `var(--mood-${value})`,
                        }}
                      />
                    </div>
                    <div className="distribution-count">
                      <span className="count-number">{count}</span>
                      <span className="count-percent">
                        {percentage.toFixed(0)}%
                      </span>
                    </div>
                  </div>
                );
              })}
            </div>
          </div>

          {/* Main Chart */}
          <MoodChart journals={journals} />

          {/* Insights */}
          <div className="dashboard-insights">
            <h2>ðŸ’¡ Insights</h2>
            <div className="insights-grid">
              <div className="insight-card">
                <h3>Overall Trend</h3>
                <p>
                  {stats.average >= 4 &&
                    "You're doing great! Keep up the positive vibes! ðŸŒŸ"}
                  {stats.average >= 3 &&
                    stats.average < 4 &&
                    "Things are looking balanced. Remember to take care of yourself. ðŸŒ±"}
                  {stats.average >= 2 &&
                    stats.average < 3 &&
                    "You might be going through a challenging time. It's okay to reach out for support. ðŸ’™"}
                  {stats.average < 2 &&
                    "Be gentle with yourself. Every day is a new opportunity. ðŸŒˆ"}
                </p>
              </div>

              <div className="insight-card">
                <h3>Total Entries</h3>
                <p>
                  You've written <strong>{stats.total}</strong> journal entries.
                  That's{" "}
                  {stats.total === 1
                    ? "a great start!"
                    : "impressive consistency! Keep reflecting!"}
                </p>
              </div>

              <div className="insight-card">
                <h3>Mood Range</h3>
                <p>
                  Your mood has ranged from {getMoodEmoji(stats.lowest)} to{" "}
                  {getMoodEmoji(stats.highest)}, showing a span of{" "}
                  {stats.highest - stats.lowest} points.
                </p>
              </div>

              <div className="insight-card">
                <h3>Best Mood</h3>
                <p>
                  Your best recorded mood was {getMoodEmoji(stats.highest)}.
                  Reflect on what made those days special!
                </p>
              </div>
            </div>
          </div>
        </>
      )}
    </div>
  );
};

export default MoodDashboard;
```

### FILE: src\components\Expenses\ExpenseCharts.css

```text
.expense-charts {
  margin-top: var(--spacing-xl);
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--spacing-lg);
}

.expense-chart-card {
  background-color: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-sm);
  padding: var(--spacing-md);
}

.expense-chart-card h3 {
  margin-bottom: var(--spacing-md);
}

.expense-chart-wrap {
  width: 100%;
  min-height: 280px;
}

@media (max-width: 1024px) {
  .expense-charts {
    grid-template-columns: 1fr;
  }
}
```

### FILE: src\components\Expenses\ExpenseCharts.jsx

```text
import { useMemo } from "react";
import {
  Bar,
  BarChart,
  CartesianGrid,
  Line,
  LineChart,
  ResponsiveContainer,
  Tooltip,
  XAxis,
  YAxis,
} from "recharts";
import "./ExpenseCharts.css";

function ExpenseCharts({ expenses }) {
  const chartData = useMemo(() => {
    const byDate = {};
    const byCategory = {};

    expenses.forEach((item) => {
      const dateKey = item.date;
      byDate[dateKey] = (byDate[dateKey] || 0) + Number(item.amount);

      const category = item.category || "Other";
      byCategory[category] = (byCategory[category] || 0) + Number(item.amount);
    });

    const dateSeries = Object.entries(byDate)
      .map(([date, amount]) => ({ date, amount }))
      .sort((a, b) => new Date(a.date) - new Date(b.date))
      .slice(-14);

    const categorySeries = Object.entries(byCategory)
      .map(([category, amount]) => ({ category, amount }))
      .sort((a, b) => b.amount - a.amount)
      .slice(0, 8);

    return { dateSeries, categorySeries };
  }, [expenses]);

  if (expenses.length === 0) {
    return null;
  }

  return (
    <section className="expense-charts">
      <div className="expense-chart-card">
        <h3>Last 14 Days Spend Trend</h3>
        <div className="expense-chart-wrap">
          <ResponsiveContainer width="100%" height={280}>
            <LineChart data={chartData.dateSeries}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="date" tick={{ fontSize: 12 }} />
              <YAxis tick={{ fontSize: 12 }} />
              <Tooltip
                formatter={(value) => [
                  `â‚¹${Number(value).toFixed(2)}`,
                  "Amount",
                ]}
              />
              <Line
                type="monotone"
                dataKey="amount"
                stroke="#2563eb"
                strokeWidth={3}
              />
            </LineChart>
          </ResponsiveContainer>
        </div>
      </div>

      <div className="expense-chart-card">
        <h3>Top Spending Categories</h3>
        <div className="expense-chart-wrap">
          <ResponsiveContainer width="100%" height={280}>
            <BarChart data={chartData.categorySeries}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="category" tick={{ fontSize: 12 }} />
              <YAxis tick={{ fontSize: 12 }} />
              <Tooltip
                formatter={(value) => [
                  `â‚¹${Number(value).toFixed(2)}`,
                  "Amount",
                ]}
              />
              <Bar dataKey="amount" fill="#16a34a" radius={[8, 8, 0, 0]} />
            </BarChart>
          </ResponsiveContainer>
        </div>
      </div>
    </section>
  );
}

export default ExpenseCharts;
```

### FILE: src\components\Expenses\ExpenseForm.css

```text
.expense-form {
  background-color: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: var(--spacing-lg);
  box-shadow: var(--shadow-sm);
}

.expense-form h2 {
  margin-bottom: var(--spacing-lg);
}

.expense-form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--spacing-md);
}

.expense-form-error {
  background-color: rgba(239, 68, 68, 0.1);
  border: 1px solid var(--danger);
  color: var(--danger);
  border-radius: var(--radius-md);
  padding: var(--spacing-sm) var(--spacing-md);
  margin-bottom: var(--spacing-md);
}

@media (max-width: 768px) {
  .expense-form-row {
    grid-template-columns: 1fr;
  }
}
```

### FILE: src\components\Expenses\ExpenseForm.jsx

```text
import { useState } from "react";
import { getTodayDateString } from "../../utils/dateHelpers";
import "./ExpenseForm.css";

const CATEGORY_OPTIONS = [
  "Food",
  "Transport",
  "Shopping",
  "Bills",
  "Health",
  "Education",
  "Entertainment",
  "Other",
];

function ExpenseForm({ onAddExpense }) {
  const [formData, setFormData] = useState({
    title: "",
    amount: "",
    category: "Food",
    date: getTodayDateString(),
    notes: "",
  });

  const [error, setError] = useState("");

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData((prev) => ({ ...prev, [name]: value }));
    if (error) {
      setError("");
    }
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    if (!formData.title.trim()) {
      setError("Expense title is required.");
      return;
    }

    const amount = Number(formData.amount);
    if (!Number.isFinite(amount) || amount <= 0) {
      setError("Please enter a valid amount greater than 0.");
      return;
    }

    await onAddExpense({
      ...formData,
      amount,
      title: formData.title.trim(),
      notes: formData.notes.trim(),
    });

    setFormData({
      title: "",
      amount: "",
      category: "Food",
      date: getTodayDateString(),
      notes: "",
    });
  };

  return (
    <form className="expense-form" onSubmit={handleSubmit}>
      <h2>ðŸ’¸ Add Daily Expense</h2>

      {error && <div className="expense-form-error">{error}</div>}

      <div className="form-group">
        <label htmlFor="expense-title">Expense Title</label>
        <input
          id="expense-title"
          type="text"
          name="title"
          value={formData.title}
          onChange={handleChange}
          placeholder="e.g., Lunch, Uber, Books"
        />
      </div>

      <div className="expense-form-row">
        <div className="form-group">
          <label htmlFor="expense-amount">Amount</label>
          <input
            id="expense-amount"
            type="number"
            min="0"
            step="0.01"
            name="amount"
            value={formData.amount}
            onChange={handleChange}
            placeholder="0.00"
          />
        </div>

        <div className="form-group">
          <label htmlFor="expense-date">Date</label>
          <input
            id="expense-date"
            type="date"
            name="date"
            value={formData.date}
            onChange={handleChange}
          />
        </div>
      </div>

      <div className="expense-form-row">
        <div className="form-group">
          <label htmlFor="expense-category">Category</label>
          <select
            id="expense-category"
            name="category"
            value={formData.category}
            onChange={handleChange}
          >
            {CATEGORY_OPTIONS.map((option) => (
              <option key={option} value={option}>
                {option}
              </option>
            ))}
          </select>
        </div>
      </div>

      <div className="form-group">
        <label htmlFor="expense-notes">Notes (Optional)</label>
        <textarea
          id="expense-notes"
          name="notes"
          value={formData.notes}
          onChange={handleChange}
          rows="3"
          placeholder="Any context for this expense..."
        />
      </div>

      <button type="submit" className="btn btn-primary">
        Save Expense
      </button>
    </form>
  );
}

export default ExpenseForm;
```

### FILE: src\components\Expenses\ExpenseList.css

```text
.expense-list {
  margin-top: var(--spacing-xl);
}

.expense-controls {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: var(--spacing-md);
  margin-bottom: var(--spacing-md);
}

.expense-results {
  color: var(--text-secondary);
  margin-bottom: var(--spacing-md);
}

.expense-cards {
  display: grid;
  gap: var(--spacing-md);
}

.expense-card {
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  background-color: var(--surface);
  padding: var(--spacing-md);
  box-shadow: var(--shadow-sm);
}

.expense-card-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: var(--spacing-md);
}

.expense-meta {
  margin: 0;
  color: var(--text-secondary);
  font-size: var(--font-size-sm);
}

.expense-amount {
  font-weight: 700;
  font-size: var(--font-size-xl);
}

.expense-notes {
  margin: var(--spacing-md) 0;
}

.expense-card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: var(--text-secondary);
  font-size: var(--font-size-sm);
}

.expense-empty-state {
  margin-top: var(--spacing-xl);
  border: 1px dashed var(--border);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  text-align: center;
}

@media (max-width: 768px) {
  .expense-controls {
    grid-template-columns: 1fr;
  }

  .expense-card-header {
    flex-direction: column;
  }
}
```

### FILE: src\components\Expenses\ExpenseList.jsx

```text
import { useMemo, useState } from "react";
import { formatDate, getRelativeDate } from "../../utils/dateHelpers";
import "./ExpenseList.css";

function ExpenseList({ expenses, onDeleteExpense }) {
  const [search, setSearch] = useState("");
  const [category, setCategory] = useState("All");

  const categories = useMemo(() => {
    const unique = new Set(expenses.map((item) => item.category));
    return ["All", ...Array.from(unique)];
  }, [expenses]);

  const filteredExpenses = useMemo(() => {
    let result = [...expenses];

    if (search.trim()) {
      const query = search.toLowerCase();
      result = result.filter(
        (item) =>
          item.title.toLowerCase().includes(query) ||
          (item.notes || "").toLowerCase().includes(query)
      );
    }

    if (category !== "All") {
      result = result.filter((item) => item.category === category);
    }

    return result.sort((a, b) => new Date(b.date) - new Date(a.date));
  }, [expenses, search, category]);

  if (expenses.length === 0) {
    return (
      <div className="expense-empty-state">
        <h3>No expenses tracked yet</h3>
        <p>Add your first expense to start monitoring your daily spend.</p>
      </div>
    );
  }

  return (
    <div className="expense-list">
      <div className="expense-controls">
        <input
          type="text"
          value={search}
          onChange={(e) => setSearch(e.target.value)}
          placeholder="Search expenses..."
        />

        <select value={category} onChange={(e) => setCategory(e.target.value)}>
          {categories.map((option) => (
            <option key={option} value={option}>
              {option}
            </option>
          ))}
        </select>
      </div>

      <div className="expense-results">{filteredExpenses.length} expense entries</div>

      <div className="expense-cards">
        {filteredExpenses.map((expense) => (
          <article className="expense-card" key={expense.id}>
            <div className="expense-card-header">
              <div>
                <h3>{expense.title}</h3>
                <p className="expense-meta">
                  {getRelativeDate(expense.date)} â€¢ {expense.category}
                </p>
              </div>
              <div className="expense-amount">â‚¹{expense.amount.toFixed(2)}</div>
            </div>

            {expense.notes && <p className="expense-notes">{expense.notes}</p>}

            <div className="expense-card-footer">
              <span>{formatDate(expense.date)}</span>
              <button
                type="button"
                className="btn btn-danger btn-sm"
                onClick={() => onDeleteExpense(expense.id)}
              >
                Delete
              </button>
            </div>
          </article>
        ))}
      </div>
    </div>
  );
}

export default ExpenseList;
```

### FILE: src\components\Journal\JournalCard.css

```text
/**
 * JournalCard Styles
 */

.journal-card {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  border: 1px solid var(--border);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  transition: all var(--transition-base);
  display: flex;
  flex-direction: column;
  animation: slideIn var(--transition-base);
  height: 100%;
}

.journal-card:hover {
  box-shadow: var(--shadow-lg);
  transform: translateY(-4px);
}

.card-header {
  padding: var(--spacing-lg);
  border-bottom: 1px solid var(--border);
  background: linear-gradient(
    135deg,
    rgba(99, 102, 241, 0.05),
    rgba(129, 140, 248, 0.05)
  );
}

.card-title-section {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: var(--spacing-md);
}

.card-title-section h3 {
  margin: 0;
  color: var(--text-primary);
  word-break: break-word;
}

.card-meta {
  display: flex;
  gap: var(--spacing-md);
  align-items: center;
  flex-shrink: 0;
}

.card-date {
  font-size: var(--font-size-xs);
  color: var(--text-secondary);
  white-space: nowrap;
}

.card-mood {
  font-size: 1.25rem;
  cursor: default;
  transition: transform var(--transition-fast);
}

.journal-card:hover .card-mood {
  transform: scale(1.2);
}

.card-content {
  padding: var(--spacing-lg);
  flex: 1;
  display: flex;
  flex-direction: column;
}

.preview-content {
  color: var(--text-secondary);
  line-height: var(--line-height-relaxed);
  margin: 0;
  font-size: var(--font-size-sm);
}

.full-content {
  color: var(--text-primary);
  line-height: var(--line-height-relaxed);
  margin: 0;
  white-space: pre-wrap;
  word-break: break-word;
}

.card-footer {
  display: flex;
  gap: var(--spacing-sm);
  padding: var(--spacing-lg);
  border-top: 1px solid var(--border);
  background-color: var(--surface-alt);
}

.card-footer .btn {
  flex: 1;
}

.card-full-info {
  padding: 0 var(--spacing-lg) var(--spacing-lg);
  border-top: 1px solid var(--border);
  font-size: var(--font-size-sm);
  color: var(--text-secondary);
  animation: slideIn var(--transition-base);
}

.card-full-info p {
  margin: var(--spacing-sm) 0;
}

.card-full-info strong {
  color: var(--text-primary);
}

.journal-card.expanded {
  grid-column: span 2;
}

@media (max-width: 768px) {
  .journal-card.expanded {
    grid-column: span 1;
  }

  .card-title-section {
    flex-direction: column;
  }

  .card-meta {
    width: 100%;
  }

  .card-footer {
    flex-direction: column;
  }
}
```

### FILE: src\components\Journal\JournalCard.jsx

```text
import { useState } from "react";
import { formatDate, getRelativeDate } from "../../utils/dateHelpers";
import { getMoodEmoji, getMoodLabel } from "../../utils/moodCalculator";
import "./JournalCard.css";

/**
 * JournalCard Component
 * Individual journal entry card
 */
const JournalCard = ({ journal, onDelete }) => {
  const [isExpanded, setIsExpanded] = useState(false);

  const handleDelete = () => {
    if (window.confirm("Are you sure you want to delete this journal entry?")) {
      onDelete(journal.id);
    }
  };

  const preview =
    journal.content.substring(0, 150) +
    (journal.content.length > 150 ? "..." : "");

  return (
    <div className={`journal-card ${isExpanded ? "expanded" : ""}`}>
      <div className="card-header">
        <div className="card-title-section">
          <h3>{journal.title}</h3>
          <div className="card-meta">
            <span className="card-date">{getRelativeDate(journal.date)}</span>
            <span className="card-mood" title={getMoodLabel(journal.mood)}>
              {getMoodEmoji(journal.mood)}
            </span>
          </div>
        </div>
      </div>

      <div className="card-content">
        {isExpanded ? (
          <p className="full-content">{journal.content}</p>
        ) : (
          <p className="preview-content">{preview}</p>
        )}
      </div>

      <div className="card-footer">
        <button
          className="btn btn-secondary btn-sm"
          onClick={() => setIsExpanded(!isExpanded)}
        >
          {isExpanded ? "ðŸ‘ˆ Collapse" : "ðŸ‘‰ Read More"}
        </button>
        <button className="btn btn-danger btn-sm" onClick={handleDelete}>
          ðŸ—‘ï¸ Delete
        </button>
      </div>

      {isExpanded && (
        <div className="card-full-info">
          <p className="full-date">
            <strong>Created:</strong> {formatDate(journal.date)}
          </p>
          <p className="full-mood">
            <strong>Mood:</strong> {getMoodEmoji(journal.mood)}{" "}
            {getMoodLabel(journal.mood)}
          </p>
        </div>
      )}
    </div>
  );
};

export default JournalCard;
```

### FILE: src\components\Journal\JournalForm.css

```text
/**
 * JournalForm Styles
 */

.journal-form {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  box-shadow: var(--shadow-md);
  max-width: 700px;
  margin: 0 auto var(--spacing-xl);
  animation: slideIn var(--transition-base);
}

.journal-form h2 {
  margin-bottom: var(--spacing-lg);
  color: var(--text-primary);
}

.form-success {
  background-color: rgba(34, 197, 94, 0.1);
  border: 1px solid var(--success);
  border-radius: var(--radius-md);
  padding: var(--spacing-md) var(--spacing-lg);
  color: var(--success);
  margin-bottom: var(--spacing-lg);
  animation: slideIn var(--transition-base);
}

.form-group {
  display: flex;
  flex-direction: column;
  margin-bottom: var(--spacing-lg);
  position: relative;
}

.form-group label {
  font-weight: 600;
  margin-bottom: var(--spacing-sm);
  color: var(--text-primary);
  font-size: var(--font-size-sm);
}

.char-count {
  position: absolute;
  bottom: 5px;
  right: 10px;
  font-size: var(--font-size-xs);
  color: var(--text-tertiary);
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--spacing-lg);
  margin-bottom: var(--spacing-lg);
}

.mood-selector {
  display: flex;
  gap: var(--spacing-md);
  justify-content: space-between;
  margin-top: var(--spacing-sm);
}

.mood-button {
  flex: 1;
  aspect-ratio: 1;
  background-color: var(--surface-alt);
  border: 2px solid transparent;
  border-radius: var(--radius-lg);
  cursor: pointer;
  transition: all var(--transition-fast);
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.mood-emoji {
  font-size: 1.5rem;
  transition: transform var(--transition-fast);
}

.mood-button:hover {
  transform: scale(1.1);
  background-color: var(--border);
}

.mood-button.active {
  border-color: var(--primary-color);
  background-color: rgba(99, 102, 241, 0.1);
  box-shadow: var(--shadow-md);
}

.mood-button.active .mood-emoji {
  transform: scale(1.3);
}

.journal-form button {
  width: 100%;
  margin-top: var(--spacing-md);
}

@media (max-width: 768px) {
  .journal-form {
    padding: var(--spacing-lg);
    margin: 0 auto var(--spacing-lg);
  }

  .form-row {
    grid-template-columns: 1fr;
  }

  .mood-selector {
    flex-wrap: wrap;
  }

  .mood-button {
    flex: 0 1 calc(20% - var(--spacing-sm));
  }
}
```

### FILE: src\components\Journal\JournalForm.jsx

```text
import { useState } from "react";
import { getTodayDateString } from "../../utils/dateHelpers";
import { MOOD_MAP } from "../../utils/moodCalculator";
import "./JournalForm.css";

/**
 * JournalForm Component
 * Form for creating a new journal entry
 */
const JournalForm = ({ onSubmit }) => {
  const [formData, setFormData] = useState({
    title: "",
    content: "",
    date: getTodayDateString(),
    mood: 3,
  });

  const [submitted, setSubmitted] = useState(false);

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData((prev) => ({
      ...prev,
      [name]: name === "mood" ? parseInt(value) : value,
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    // Validation
    if (!formData.title.trim()) {
      alert("Please enter a title for your journal entry");
      return;
    }

    if (!formData.content.trim()) {
      alert("Please write something in your journal");
      return;
    }

    // Submit
    onSubmit(formData);

    // Reset form
    setFormData({
      title: "",
      content: "",
      date: getTodayDateString(),
      mood: 3,
    });

    setSubmitted(true);
    setTimeout(() => setSubmitted(false), 3000);
  };

  return (
    <form className="journal-form" onSubmit={handleSubmit}>
      <h2>âœï¸ Write Your Journal</h2>

      {submitted && (
        <div className="form-success">âœ¨ Journal entry saved successfully!</div>
      )}

      <div className="form-group">
        <label htmlFor="title">Title</label>
        <input
          type="text"
          id="title"
          name="title"
          value={formData.title}
          onChange={handleChange}
          placeholder="What's on your mind today?"
          maxLength={100}
        />
        <span className="char-count">{formData.title.length}/100</span>
      </div>

      <div className="form-row">
        <div className="form-group">
          <label htmlFor="date">Date</label>
          <input
            type="date"
            id="date"
            name="date"
            value={formData.date}
            onChange={handleChange}
          />
        </div>

        <div className="form-group">
          <label htmlFor="mood">Mood</label>
          <div className="mood-selector">
            {Object.entries(MOOD_MAP).map(([value, emoji]) => (
              <button
                key={value}
                type="button"
                className={`mood-button ${
                  formData.mood === parseInt(value) ? "active" : ""
                }`}
                onClick={() =>
                  setFormData((prev) => ({ ...prev, mood: parseInt(value) }))
                }
                title={`Mood level ${value}`}
              >
                <span className="mood-emoji">{emoji}</span>
              </button>
            ))}
          </div>
        </div>
      </div>

      <div className="form-group">
        <label htmlFor="content">Your Entry</label>
        <textarea
          id="content"
          name="content"
          value={formData.content}
          onChange={handleChange}
          placeholder="Write your thoughts, feelings, and experiences here..."
          maxLength={2000}
        />
        <span className="char-count">{formData.content.length}/2000</span>
      </div>

      <button type="submit" className="btn btn-primary btn-lg">
        ðŸ’¾ Save Entry
      </button>
    </form>
  );
};

export default JournalForm;
```

### FILE: src\components\Journal\JournalList.css

```text
/**
 * JournalList Styles
 */

.journal-list-container {
  max-width: 1000px;
  margin: 0 auto;
}

.journal-list-container h2 {
  margin-bottom: var(--spacing-xl);
  color: var(--text-primary);
}

.list-controls {
  display: flex;
  gap: var(--spacing-lg);
  margin-bottom: var(--spacing-lg);
  flex-wrap: wrap;
}

.search-box {
  flex: 1;
  min-width: 250px;
  position: relative;
  display: flex;
  align-items: center;
}

.search-icon {
  position: absolute;
  left: var(--spacing-md);
  font-size: 1.125rem;
  pointer-events: none;
}

.search-input {
  width: 100%;
  padding: var(--spacing-sm) var(--spacing-md) var(--spacing-sm)
    var(--spacing-xl) !important;
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  font-size: var(--font-size-sm);
}

.search-input::placeholder {
  color: var(--text-tertiary);
}

.filter-group {
  display: flex;
  gap: var(--spacing-md);
}

.filter-select {
  padding: var(--spacing-sm) var(--spacing-md) !important;
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  background-color: var(--surface);
  font-size: var(--font-size-sm);
  min-width: 150px;
  cursor: pointer;
}

.results-info {
  color: var(--text-secondary);
  font-size: var(--font-size-sm);
  margin-bottom: var(--spacing-md);
  padding: 0 var(--spacing-md);
}

.journals-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: var(--spacing-lg);
  animation: fadeIn var(--transition-base);
}

@media (max-width: 768px) {
  .list-controls {
    flex-direction: column;
  }

  .search-box {
    min-width: 100%;
  }

  .filter-group {
    flex-direction: column;
  }

  .filter-select {
    min-width: 100%;
  }

  .journals-grid {
    grid-template-columns: 1fr;
  }
}
```

### FILE: src\components\Journal\JournalList.jsx

```text
import { useState } from "react";
import JournalCard from "./JournalCard";
import "./JournalList.css";

/**
 * JournalList Component
 * Display list of journals with filtering and sorting
 */
const JournalList = ({ journals, onDelete }) => {
  const [sortBy, setSortBy] = useState("newest");
  const [moodFilter, setMoodFilter] = useState("all");
  const [searchQuery, setSearchQuery] = useState("");

  // Filter and sort journals
  let filteredJournals = [...journals];

  // Apply mood filter
  if (moodFilter !== "all") {
    filteredJournals = filteredJournals.filter(
      (journal) => journal.mood === parseInt(moodFilter)
    );
  }

  // Apply search filter
  if (searchQuery.trim()) {
    const query = searchQuery.toLowerCase();
    filteredJournals = filteredJournals.filter(
      (journal) =>
        journal.title.toLowerCase().includes(query) ||
        journal.content.toLowerCase().includes(query)
    );
  }

  // Apply sorting
  if (sortBy === "newest") {
    filteredJournals.sort((a, b) => new Date(b.date) - new Date(a.date));
  } else if (sortBy === "oldest") {
    filteredJournals.sort((a, b) => new Date(a.date) - new Date(b.date));
  } else if (sortBy === "mood-high") {
    filteredJournals.sort((a, b) => b.mood - a.mood);
  } else if (sortBy === "mood-low") {
    filteredJournals.sort((a, b) => a.mood - b.mood);
  }

  return (
    <div className="journal-list-container">
      <h2>ðŸ“š Your Journals</h2>

      {journals.length === 0 ? (
        <div className="empty-state">
          <h3>No journals yet</h3>
          <p>Start by creating your first journal entry to see it here.</p>
        </div>
      ) : (
        <>
          {/* Filters and Search */}
          <div className="list-controls">
            <div className="search-box">
              <span className="search-icon">ðŸ”</span>
              <input
                type="text"
                placeholder="Search journals..."
                value={searchQuery}
                onChange={(e) => setSearchQuery(e.target.value)}
                className="search-input"
              />
            </div>

            <div className="filter-group">
              <select
                value={moodFilter}
                onChange={(e) => setMoodFilter(e.target.value)}
                className="filter-select"
              >
                <option value="all">All Moods</option>
                <option value="1">ðŸ˜¡ Terrible</option>
                <option value="2">ðŸ˜• Bad</option>
                <option value="3">ðŸ˜ Neutral</option>
                <option value="4">ðŸ˜Š Good</option>
                <option value="5">ðŸ˜„ Excellent</option>
              </select>

              <select
                value={sortBy}
                onChange={(e) => setSortBy(e.target.value)}
                className="filter-select"
              >
                <option value="newest">Newest First</option>
                <option value="oldest">Oldest First</option>
                <option value="mood-high">Highest Mood</option>
                <option value="mood-low">Lowest Mood</option>
              </select>
            </div>
          </div>

          {/* Results count */}
          <div className="results-info">
            Showing {filteredJournals.length} of {journals.length} entries
          </div>

          {/* Journals List */}
          {filteredJournals.length === 0 ? (
            <div className="empty-state">
              <h3>No matches found</h3>
              <p>Try adjusting your filters or search query.</p>
            </div>
          ) : (
            <div className="journals-grid">
              {filteredJournals.map((journal) => (
                <JournalCard
                  key={journal.id}
                  journal={journal}
                  onDelete={onDelete}
                />
              ))}
            </div>
          )}
        </>
      )}
    </div>
  );
};

export default JournalList;
```

### FILE: src\components\Layout\Footer.css

```text
/**
 * Footer Styles
 */

.footer {
  background-color: #000000;
  border-top: 1px solid #333333;
  margin-top: var(--spacing-2xl);
  padding: var(--spacing-2xl) 0;
  text-align: center;
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-md);
}

.footer-content p {
  color: #ffffff;
  font-size: var(--font-size-sm);
  margin: 0.5rem 0;
}

.footer-subtitle {
  color: #cccccc;
  font-style: italic;
  font-size: var(--font-size-xs);
  margin-top: var(--spacing-sm);
}

@media (max-width: 768px) {
  .footer {
    padding: var(--spacing-lg) 0;
  }

  .footer-content {
    padding: 0 var(--spacing-md);
  }
}
```

### FILE: src\components\Layout\Footer.jsx

```text
import "./Footer.css";

/**
 * Footer Component
 * Application footer
 */
const Footer = () => {
  const currentYear = new Date().getFullYear();

  return (
    <footer className="footer">
      <div className="footer-content">
        <p>&copy; {currentYear} Daily Journal. Built with React & â¤ï¸</p>
        <p className="footer-subtitle">
          Your personal space for reflection and growth.
        </p>
      </div>
    </footer>
  );
};

export default Footer;
```

### FILE: src\components\Layout\Header.css

```text
/**
 * Header Styles
 */

.header {
  background-color: #000000;
  border-bottom: 1px solid #333333;
  box-shadow: var(--shadow-sm);
  position: sticky;
  top: 0;
  z-index: 100;
}

.header-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-md);
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 70px;
}

.header-logo {
  display: flex;
  align-items: center;
  gap: var(--spacing-sm);
  font-size: var(--font-size-xl);
  font-weight: 700;
  color: #ffffff;
  transition: transform var(--transition-fast);
}

.header-logo:hover {
  transform: scale(1.05);
  text-decoration: none;
}

.logo-emoji {
  font-size: 1.75rem;
}

.logo-text {
  background: linear-gradient(
    135deg,
    var(--primary-color),
    var(--primary-light)
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.header-nav {
  display: flex;
  gap: var(--spacing-xl);
  align-items: center;
}

.nav-link {
  color: #ffffff;
  font-weight: 500;
  transition: color var(--transition-fast);
  position: relative;
}

.nav-link:hover {
  color: var(--primary-color);
  text-decoration: none;
}

.nav-link::after {
  content: "";
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 0;
  height: 2px;
  background-color: var(--primary-color);
  transition: width var(--transition-fast);
}

.nav-link:hover::after {
  width: 100%;
}

@media (max-width: 768px) {
  .header-container {
    height: 60px;
  }

  .logo-text {
    display: none;
  }

  .header-nav {
    gap: var(--spacing-md);
    overflow-x: auto;
    scrollbar-width: none;
  }

  .header-nav::-webkit-scrollbar {
    display: none;
  }

  .nav-link {
    font-size: var(--font-size-sm);
  }
}
```

### FILE: src\components\Layout\Header.jsx

```text
import { Link } from "react-router-dom";
import "./Header.css";

/**
 * Header Component
 * Main navigation and branding
 */
const Header = () => {
  return (
    <header className="header">
      <div className="header-container">
        <Link to="/" className="header-logo">
          <span className="logo-emoji">ðŸ“”</span>
          <span className="logo-text">Daily Journal</span>
        </Link>

        <nav className="header-nav">
          <Link to="/" className="nav-link">
            Home
          </Link>
          <Link to="/journals" className="nav-link">
            Journals
          </Link>
          <Link to="/learning" className="nav-link">
            Learning
          </Link>
          <Link to="/expenses" className="nav-link">
            Expenses
          </Link>
          <Link to="/dashboard" className="nav-link">
            Dashboard
          </Link>
        </nav>
      </div>
    </header>
  );
};

export default Header;
```

### FILE: src\components\Learning\LearningCard.css

```text
/**
 * LearningCard Styles
 */

.learning-card {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  border: 1px solid var(--border);
  overflow: hidden;
  margin-bottom: var(--spacing-md);
  transition: box-shadow var(--transition-fast),
    border-color var(--transition-fast);
}

.learning-card:hover {
  box-shadow: var(--shadow-md);
  border-color: var(--primary-light);
}

.learning-card.editing {
  box-shadow: var(--shadow-lg);
  border-color: var(--primary-color);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: var(--spacing-lg);
  background-color: var(--surface-alt);
  cursor: pointer;
}

.header-left {
  display: flex;
  align-items: center;
  gap: var(--spacing-md);
  flex: 1;
}

.header-right {
  display: flex;
  align-items: center;
  gap: var(--spacing-md);
}

.card-title {
  font-size: var(--font-size-lg);
  color: var(--text-primary);
  margin: 0;
  font-weight: 600;
  flex: 1;
  transition: color var(--transition-fast);
}

.card-title:hover {
  color: var(--primary-color);
}

.category-badge {
  display: inline-block;
  padding: var(--spacing-xs) var(--spacing-sm);
  border-radius: var(--radius-full);
  color: white;
  font-size: var(--font-size-xs);
  font-weight: 600;
  white-space: nowrap;
}

.card-date {
  color: var(--text-secondary);
  font-size: var(--font-size-sm);
  white-space: nowrap;
}

.btn-toggle {
  background: none;
  border: none;
  color: var(--text-secondary);
  font-size: var(--font-size-lg);
  cursor: pointer;
  padding: 0;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: color var(--transition-fast);
}

.btn-toggle:hover {
  color: var(--primary-color);
}

.card-content {
  padding: var(--spacing-lg);
  border-top: 1px solid var(--border);
  animation: slideDown 200ms ease-out;
}

@keyframes slideDown {
  from {
    opacity: 0;
    max-height: 0;
  }
  to {
    opacity: 1;
    max-height: 1000px;
  }
}

.card-description {
  color: var(--text-primary);
  line-height: var(--line-height-relaxed);
  margin: 0 0 var(--spacing-md) 0;
  white-space: pre-wrap;
  word-wrap: break-word;
}

.card-link {
  background-color: var(--surface-alt);
  padding: var(--spacing-md);
  border-radius: var(--radius-md);
  margin-bottom: var(--spacing-md);
}

.card-link strong {
  display: block;
  color: var(--text-primary);
  margin-bottom: var(--spacing-xs);
  font-size: var(--font-size-sm);
}

.resource-link {
  display: flex;
  align-items: center;
  gap: var(--spacing-xs);
  color: var(--primary-color);
  text-decoration: none;
  font-weight: 500;
  word-break: break-all;
  transition: color var(--transition-fast);
}

.resource-link:hover {
  color: var(--primary-dark);
  text-decoration: underline;
}

.link-icon {
  margin-left: var(--spacing-xs);
  font-size: var(--font-size-sm);
}

.card-actions {
  display: flex;
  gap: var(--spacing-md);
  margin-top: var(--spacing-md);
}

.btn-edit,
.btn-delete,
.btn-save,
.btn-cancel {
  padding: var(--spacing-sm) var(--spacing-md);
  border: none;
  border-radius: var(--radius-md);
  font-size: var(--font-size-sm);
  font-weight: 600;
  cursor: pointer;
  transition: all var(--transition-fast);
}

.btn-edit {
  background-color: var(--info);
  color: white;
}

.btn-edit:hover {
  background-color: #2563eb;
}

.btn-delete {
  background-color: var(--danger);
  color: white;
}

.btn-delete:hover {
  background-color: #dc2626;
}

.btn-save {
  background: linear-gradient(135deg, var(--success), #16a34a);
  color: white;
}

.btn-save:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

.btn-cancel {
  background-color: var(--text-secondary);
  color: white;
}

.btn-cancel:hover {
  background-color: #475569;
}

.edit-buttons {
  display: flex;
  gap: var(--spacing-md);
  margin-top: var(--spacing-md);
}

.edit-input-title,
.edit-input,
.edit-textarea,
.edit-select-category {
  padding: var(--spacing-sm) var(--spacing-md);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  font-family: inherit;
  font-size: var(--font-size-base);
  color: var(--text-primary);
  background-color: var(--surface);
  margin-bottom: var(--spacing-md);
}

.edit-input-title {
  font-size: var(--font-size-lg);
  font-weight: 600;
}

.edit-select-category {
  margin-bottom: var(--spacing-md);
}

.edit-input:focus,
.edit-textarea:focus,
.edit-select-category:focus,
.edit-input-title:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
}

@media (max-width: 768px) {
  .card-header {
    flex-direction: column;
    align-items: flex-start;
    gap: var(--spacing-md);
  }

  .header-right {
    width: 100%;
    justify-content: space-between;
  }

  .card-title {
    font-size: var(--font-size-base);
  }

  .card-actions {
    flex-direction: column;
  }

  .btn-edit,
  .btn-delete,
  .btn-save,
  .btn-cancel {
    width: 100%;
  }
}
```

### FILE: src\components\Learning\LearningCard.jsx

```text
import { useState } from "react";
import "./LearningCard.css";

/**
 * LearningCard Component
 * Displays individual learning entries
 */
function LearningCard({ learning, onDelete, onUpdate }) {
  const [isExpanded, setIsExpanded] = useState(false);
  const [isEditing, setIsEditing] = useState(false);
  const [editData, setEditData] = useState(learning);

  const formatDate = (isoString) => {
    const date = new Date(isoString);
    return date.toLocaleDateString("en-US", {
      year: "numeric",
      month: "short",
      day: "numeric",
      hour: "2-digit",
      minute: "2-digit",
    });
  };

  const getCategoryColor = (category) => {
    const colors = {
      Technology: "#818cf8",
      Business: "#f97316",
      Health: "#22c55e",
      "Personal Development": "#3b82f6",
      Science: "#a855f7",
      General: "#64748b",
      Other: "#94a3b8",
    };
    return colors[category] || colors.General;
  };

  const handleEditChange = (e) => {
    const { name, value } = e.target;
    setEditData((prev) => ({
      ...prev,
      [name]: value,
    }));
  };

  const handleSaveEdit = () => {
    onUpdate(learning.id, editData);
    setIsEditing(false);
  };

  const handleDeleteClick = () => {
    if (
      window.confirm("Are you sure you want to delete this learning entry?")
    ) {
      onDelete(learning.id);
    }
  };

  if (isEditing) {
    return (
      <div className="learning-card editing">
        <div className="card-header">
          <input
            type="text"
            name="title"
            value={editData.title}
            onChange={handleEditChange}
            className="edit-input-title"
          />
          <select
            name="category"
            value={editData.category}
            onChange={handleEditChange}
            className="edit-select-category"
          >
            <option value="General">General</option>
            <option value="Technology">Technology</option>
            <option value="Business">Business</option>
            <option value="Health">Health</option>
            <option value="Personal Development">Personal Development</option>
            <option value="Science">Science</option>
            <option value="Other">Other</option>
          </select>
        </div>

        <textarea
          name="description"
          value={editData.description}
          onChange={handleEditChange}
          className="edit-textarea"
          rows="4"
        />

        <input
          type="text"
          name="link"
          value={editData.link}
          onChange={handleEditChange}
          placeholder="https://example.com"
          className="edit-input"
        />

        <div className="edit-buttons">
          <button className="btn-save" onClick={handleSaveEdit}>
            Save
          </button>
          <button
            className="btn-cancel"
            onClick={() => {
              setIsEditing(false);
              setEditData(learning);
            }}
          >
            Cancel
          </button>
        </div>
      </div>
    );
  }

  return (
    <div className="learning-card">
      <div className="card-header">
        <div className="header-left">
          <h3 className="card-title" onClick={() => setIsExpanded(!isExpanded)}>
            {learning.title}
          </h3>
          <span
            className="category-badge"
            style={{ backgroundColor: getCategoryColor(learning.category) }}
          >
            {learning.category}
          </span>
        </div>
        <div className="header-right">
          <span className="card-date">{formatDate(learning.createdAt)}</span>
          <button
            className="btn-toggle"
            onClick={() => setIsExpanded(!isExpanded)}
          >
            {isExpanded ? "â–¼" : "â–¶"}
          </button>
        </div>
      </div>

      {isExpanded && (
        <div className="card-content">
          <p className="card-description">{learning.description}</p>

          {learning.link && (
            <div className="card-link">
              <strong>Resource Link:</strong>
              <a
                href={learning.link}
                target="_blank"
                rel="noopener noreferrer"
                className="resource-link"
              >
                {learning.link}
                <span className="link-icon">ðŸ”—</span>
              </a>
            </div>
          )}

          <div className="card-actions">
            <button
              className="btn-edit"
              onClick={() => setIsEditing(true)}
              title="Edit"
            >
              âœï¸ Edit
            </button>
            <button
              className="btn-delete"
              onClick={handleDeleteClick}
              title="Delete"
            >
              ðŸ—‘ï¸ Delete
            </button>
          </div>
        </div>
      )}
    </div>
  );
}

export default LearningCard;
```

### FILE: src\components\Learning\LearningForm.css

```text
/**
 * LearningForm Styles
 */

.learning-form {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  box-shadow: var(--shadow-md);
  margin-bottom: var(--spacing-xl);
  max-width: 600px;
}

.form-title {
  font-size: var(--font-size-2xl);
  color: var(--text-primary);
  margin-bottom: var(--spacing-lg);
  font-weight: 700;
}

.form-group {
  margin-bottom: var(--spacing-lg);
  display: flex;
  flex-direction: column;
}

.form-group label {
  font-weight: 600;
  margin-bottom: var(--spacing-sm);
  color: var(--text-primary);
  font-size: var(--font-size-sm);
}

.form-group input,
.form-group textarea,
.form-group select {
  padding: var(--spacing-sm) var(--spacing-md);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  font-family: inherit;
  font-size: var(--font-size-base);
  color: var(--text-primary);
  background-color: var(--surface);
  transition: border-color var(--transition-fast),
    box-shadow var(--transition-fast);
}

.form-group input:focus,
.form-group textarea:focus,
.form-group select:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
}

.form-group input.error,
.form-group textarea.error {
  border-color: var(--danger);
  background-color: rgba(239, 68, 68, 0.05);
}

.error-text {
  color: var(--danger);
  font-size: var(--font-size-xs);
  margin-top: var(--spacing-xs);
}

.btn-submit {
  background: linear-gradient(
    135deg,
    var(--primary-color),
    var(--primary-dark)
  );
  color: white;
  padding: var(--spacing-md) var(--spacing-xl);
  border: none;
  border-radius: var(--radius-md);
  font-size: var(--font-size-base);
  font-weight: 600;
  cursor: pointer;
  transition: transform var(--transition-fast),
    box-shadow var(--transition-fast);
}

.btn-submit:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-lg);
}

.btn-submit:active {
  transform: translateY(0);
}

@media (max-width: 768px) {
  .learning-form {
    padding: var(--spacing-lg);
  }

  .form-title {
    font-size: var(--font-size-xl);
  }
}
```

### FILE: src\components\Learning\LearningForm.jsx

```text
import { useState } from "react";
import "./LearningForm.css";

/**
 * LearningForm Component
 * Form to add new learning entries with links and descriptions
 */
function LearningForm({ onAddLearning }) {
  const [formData, setFormData] = useState({
    title: "",
    description: "",
    link: "",
    category: "General",
  });

  const [errors, setErrors] = useState({});

  const categories = ["General", "AI", "React", "Pega", "Others"];

  const validateForm = () => {
    const newErrors = {};

    if (!formData.title.trim()) {
      newErrors.title = "Title is required";
    }

    if (!formData.description.trim()) {
      newErrors.description = "Description is required";
    }

    if (formData.link && !isValidUrl(formData.link)) {
      newErrors.link = "Please enter a valid URL";
    }

    return newErrors;
  };

  const isValidUrl = (string) => {
    try {
      new URL(string);
      return true;
    } catch (_) {
      return false;
    }
  };

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData((prev) => ({
      ...prev,
      [name]: value,
    }));
    if (errors[name]) {
      setErrors((prev) => ({
        ...prev,
        [name]: "",
      }));
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const newErrors = validateForm();

    if (Object.keys(newErrors).length > 0) {
      setErrors(newErrors);
      return;
    }

    const learningEntry = {
      id: Date.now().toString(),
      ...formData,
      createdAt: new Date().toISOString(),
    };

    onAddLearning(learningEntry);

    setFormData({
      title: "",
      description: "",
      link: "",
      category: "General",
    });

    setErrors({});
  };

  return (
    <form className="learning-form" onSubmit={handleSubmit}>
      <h2 className="form-title">ðŸ“š What I Learned Today</h2>

      <div className="form-group">
        <label htmlFor="title">Learning Title</label>
        <input
          type="text"
          id="title"
          name="title"
          value={formData.title}
          onChange={handleChange}
          placeholder="e.g., React Hooks Best Practices"
          className={errors.title ? "error" : ""}
        />
        {errors.title && <span className="error-text">{errors.title}</span>}
      </div>

      <div className="form-group">
        <label htmlFor="category">Category</label>
        <select
          id="category"
          name="category"
          value={formData.category}
          onChange={handleChange}
        >
          {categories.map((cat) => (
            <option key={cat} value={cat}>
              {cat}
            </option>
          ))}
        </select>
      </div>

      <div className="form-group">
        <label htmlFor="description">Description</label>
        <textarea
          id="description"
          name="description"
          value={formData.description}
          onChange={handleChange}
          placeholder="Describe what you learned today..."
          rows="4"
          className={errors.description ? "error" : ""}
        />
        {errors.description && (
          <span className="error-text">{errors.description}</span>
        )}
      </div>

      <div className="form-group">
        <label htmlFor="link">Resource Link (Optional)</label>
        <input
          type="text"
          id="link"
          name="link"
          value={formData.link}
          onChange={handleChange}
          placeholder="https://example.com"
          className={errors.link ? "error" : ""}
        />
        {errors.link && <span className="error-text">{errors.link}</span>}
      </div>

      <button type="submit" className="btn-submit">
        Add Learning Entry
      </button>
    </form>
  );
}

export default LearningForm;
```

### FILE: src\components\Learning\LearningList.css

```text
/**
 * LearningList Styles
 */

.learning-list-container {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  margin-bottom: var(--spacing-xl);
}

.learning-controls {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: var(--spacing-lg);
  margin-bottom: var(--spacing-xl);
  align-items: center;
}

.search-box {
  flex: 1;
}

.search-input {
  width: 100%;
  padding: var(--spacing-md) var(--spacing-lg);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  font-size: var(--font-size-base);
  background-color: var(--surface);
  color: var(--text-primary);
  transition: border-color var(--transition-fast),
    box-shadow var(--transition-fast);
}

.search-input:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
}

.search-input::placeholder {
  color: var(--text-tertiary);
}

.filter-controls {
  display: flex;
  gap: var(--spacing-md);
}

.filter-select,
.sort-select {
  padding: var(--spacing-md) var(--spacing-lg);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  font-size: var(--font-size-base);
  background-color: var(--surface);
  color: var(--text-primary);
  cursor: pointer;
  transition: border-color var(--transition-fast);
}

.filter-select:focus,
.sort-select:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
}

.empty-state {
  text-align: center;
  padding: var(--spacing-2xl) var(--spacing-lg);
}

.empty-message {
  color: var(--text-secondary);
  font-size: var(--font-size-base);
  margin: 0;
}

.learning-cards {
  animation: fadeIn 300ms ease-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.results-count {
  color: var(--text-secondary);
  font-size: var(--font-size-sm);
  margin-bottom: var(--spacing-lg);
  padding-bottom: var(--spacing-md);
  border-bottom: 1px solid var(--border);
}

@media (max-width: 768px) {
  .learning-list-container {
    padding: var(--spacing-lg);
  }

  .learning-controls {
    grid-template-columns: 1fr;
  }

  .filter-controls {
    flex-direction: column;
  }

  .filter-select,
  .sort-select {
    width: 100%;
  }
}
```

### FILE: src\components\Learning\LearningList.jsx

```text
import { useState, useMemo } from "react";
import LearningCard from "./LearningCard";
import "./LearningList.css";

/**
 * LearningList Component
 * Displays all learning entries with filtering and search capabilities
 */
function LearningList({ learnings, onDelete, onUpdate }) {
  const [searchTerm, setSearchTerm] = useState("");
  const [filterCategory, setFilterCategory] = useState("All");
  const [sortBy, setSortBy] = useState("newest");

  const categories = [
    "All",
    "General",
    "Technology",
    "Business",
    "Health",
    "Personal Development",
    "Science",
    "Other",
  ];

  const filteredAndSortedLearnings = useMemo(() => {
    let filtered = learnings;

    // Filter by search term
    if (searchTerm.trim()) {
      const term = searchTerm.toLowerCase();
      filtered = filtered.filter(
        (item) =>
          item.title.toLowerCase().includes(term) ||
          item.description.toLowerCase().includes(term)
      );
    }

    // Filter by category
    if (filterCategory !== "All") {
      filtered = filtered.filter((item) => item.category === filterCategory);
    }

    // Sort
    const sorted = [...filtered];
    if (sortBy === "newest") {
      sorted.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
    } else if (sortBy === "oldest") {
      sorted.sort((a, b) => new Date(a.createdAt) - new Date(b.createdAt));
    } else if (sortBy === "title") {
      sorted.sort((a, b) => a.title.localeCompare(b.title));
    }

    return sorted;
  }, [learnings, searchTerm, filterCategory, sortBy]);

  return (
    <div className="learning-list-container">
      <div className="learning-controls">
        <div className="search-box">
          <input
            type="text"
            placeholder="Search learnings..."
            value={searchTerm}
            onChange={(e) => setSearchTerm(e.target.value)}
            className="search-input"
          />
        </div>

        <div className="filter-controls">
          <select
            value={filterCategory}
            onChange={(e) => setFilterCategory(e.target.value)}
            className="filter-select"
          >
            {categories.map((cat) => (
              <option key={cat} value={cat}>
                {cat === "All" ? "All Categories" : cat}
              </option>
            ))}
          </select>

          <select
            value={sortBy}
            onChange={(e) => setSortBy(e.target.value)}
            className="sort-select"
          >
            <option value="newest">Newest First</option>
            <option value="oldest">Oldest First</option>
            <option value="title">Title (A-Z)</option>
          </select>
        </div>
      </div>

      {filteredAndSortedLearnings.length === 0 ? (
        <div className="empty-state">
          <p className="empty-message">
            {learnings.length === 0
              ? "No learning entries yet. Start by adding your first learning! ðŸ“š"
              : "No results found. Try adjusting your filters."}
          </p>
        </div>
      ) : (
        <div className="learning-cards">
          <p className="results-count">
            {filteredAndSortedLearnings.length} learning{" "}
            {filteredAndSortedLearnings.length !== 1 ? "entries" : "entry"}{" "}
            found
          </p>
          {filteredAndSortedLearnings.map((learning) => (
            <LearningCard
              key={learning.id}
              learning={learning}
              onDelete={onDelete}
              onUpdate={onUpdate}
            />
          ))}
        </div>
      )}
    </div>
  );
}

export default LearningList;
```

### FILE: src\hooks\useExpenses.js

```text
import { useEffect, useState } from "react";
import {
  STORE_NAMES,
  deleteItem,
  getAllItems,
  putItem,
  putManyItems,
} from "../utils/indexedDb";

export function useExpenses() {
  const [expenses, setExpenses] = useState([]);

  useEffect(() => {
    const loadExpenses = async () => {
      try {
        let loaded = await getAllItems(STORE_NAMES.expenses);

        // One-time migration from localStorage to IndexedDB.
        if (loaded.length === 0) {
          const local = window.localStorage.getItem("expenses");
          if (local) {
            const parsed = JSON.parse(local);
            if (Array.isArray(parsed) && parsed.length > 0) {
              await putManyItems(STORE_NAMES.expenses, parsed);
              loaded = parsed;
            }
          }
        }

        loaded.sort((a, b) => new Date(b.date) - new Date(a.date));
        setExpenses(loaded);
      } catch (error) {
        console.error("Error loading expenses:", error);
      }
    };

    loadExpenses();
  }, []);

  const addExpense = async (expenseData) => {
    const expense = {
      id: Date.now().toString(),
      createdAt: new Date().toISOString(),
      ...expenseData,
      amount: Number(expenseData.amount),
    };

    try {
      await putItem(STORE_NAMES.expenses, expense);
      setExpenses((prev) => [expense, ...prev]);
    } catch (error) {
      console.error("Error adding expense:", error);
    }

    return expense;
  };

  const deleteExpense = async (id) => {
    try {
      await deleteItem(STORE_NAMES.expenses, id);
      setExpenses((prev) => prev.filter((item) => item.id !== id));
    } catch (error) {
      console.error("Error deleting expense:", error);
    }
  };

  return {
    expenses,
    addExpense,
    deleteExpense,
  };
}
```

### FILE: src\hooks\useLearnings.js

```text
import { useEffect, useState } from "react";
import {
  STORE_NAMES,
  deleteItem,
  getAllItems,
  putItem,
  putManyItems,
} from "../utils/indexedDb";

/**
 * Custom Hook for managing Learning entries with Local Storage
 */
export function useLearnings() {
  const [learnings, setLearnings] = useState([]);

  useEffect(() => {
    const loadLearnings = async () => {
      try {
        let loaded = await getAllItems(STORE_NAMES.learnings);

        // One-time migration from localStorage to IndexedDB.
        if (loaded.length === 0) {
          const local = window.localStorage.getItem("learnings");
          if (local) {
            const parsed = JSON.parse(local);
            if (Array.isArray(parsed) && parsed.length > 0) {
              await putManyItems(STORE_NAMES.learnings, parsed);
              loaded = parsed;
            }
          }
        }

        loaded.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
        setLearnings(loaded);
      } catch (error) {
        console.error("Error loading learnings:", error);
      }
    };

    loadLearnings();
  }, []);

  const addLearning = async (learning) => {
    try {
      await putItem(STORE_NAMES.learnings, learning);
      setLearnings((prev) => [learning, ...prev]);
    } catch (error) {
      console.error("Error adding learning:", error);
    }
  };

  const deleteLearning = async (id) => {
    try {
      await deleteItem(STORE_NAMES.learnings, id);
      setLearnings((prev) => prev.filter((item) => item.id !== id));
    } catch (error) {
      console.error("Error deleting learning:", error);
    }
  };

  const updateLearning = async (id, updatedData) => {
    const existing = learnings.find((item) => item.id === id);
    if (!existing) {
      return;
    }

    const updated = { ...existing, ...updatedData };

    try {
      await putItem(STORE_NAMES.learnings, updated);
      setLearnings((prev) =>
        prev.map((item) => (item.id === id ? updated : item))
      );
    } catch (error) {
      console.error("Error updating learning:", error);
    }
  };

  return {
    learnings,
    addLearning,
    deleteLearning,
    updateLearning,
  };
}
```

### FILE: src\hooks\useLocalStorage.js

```text
/**
 * Custom React Hook for Local Storage
 * Persists state to browser Local Storage
 */

import { useEffect, useState } from "react";
import {
  STORE_NAMES,
  deleteItem,
  getAllItems,
  putItem,
  putManyItems,
} from "../utils/indexedDb";

/**
 * Custom hook for managing state with Local Storage persistence
 * @param {string} key - Local Storage key
 * @param {*} initialValue - Initial value if key doesn't exist
 * @returns {[*, Function]} State value and setter function
 */
export const useLocalStorage = (key, initialValue) => {
  // State to store our value
  const [storedValue, setStoredValue] = useState(() => {
    try {
      // Get from local storage by key
      const item = window.localStorage.getItem(key);
      console.log(
        `[LocalStorage] Loading key "${key}":`,
        item ? JSON.parse(item) : "NOT FOUND - using initial value"
      );
      // Parse stored json or if none return initialValue
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(`Error reading from localStorage for key "${key}":`, error);
      return initialValue;
    }
  });

  // Return a wrapped version of useState's setter function that
  // persists the new value to localStorage
  const setValue = (value) => {
    try {
      // Allow value to be a function so we have same API as useState
      const valueToStore =
        value instanceof Function ? value(storedValue) : value;
      // Save state
      setStoredValue(valueToStore);
      // Save to local storage
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(`Error writing to localStorage for key "${key}":`, error);
    }
  };

  return [storedValue, setValue];
};

/**
 * Custom hook for managing journal entries in Local Storage
 * @returns {Object} Object with journals and methods to manage them
 */
export const useJournals = () => {
  const [journals, setJournals] = useState([]);

  useEffect(() => {
    const loadJournals = async () => {
      try {
        let loaded = await getAllItems(STORE_NAMES.journals);

        // One-time migration from localStorage to IndexedDB.
        if (loaded.length === 0) {
          const local = window.localStorage.getItem("journals");
          if (local) {
            const parsed = JSON.parse(local);
            if (Array.isArray(parsed) && parsed.length > 0) {
              await putManyItems(STORE_NAMES.journals, parsed);
              loaded = parsed;
            }
          }
        }

        loaded.sort((a, b) => new Date(b.date) - new Date(a.date));
        setJournals(loaded);
      } catch (error) {
        console.error("Error loading journals from IndexedDB:", error);
      }
    };

    loadJournals();
  }, []);

  const addJournal = async (journal) => {
    const newJournal = {
      id: Date.now().toString(),
      createdAt: new Date().toISOString(),
      ...journal,
    };

    try {
      await putItem(STORE_NAMES.journals, newJournal);
      setJournals((prev) => [newJournal, ...prev]);
    } catch (error) {
      console.error("Error adding journal:", error);
    }

    return newJournal;
  };

  const deleteJournal = async (id) => {
    try {
      await deleteItem(STORE_NAMES.journals, id);
      setJournals((prev) => prev.filter((journal) => journal.id !== id));
    } catch (error) {
      console.error("Error deleting journal:", error);
    }
  };

  const updateJournal = async (id, updates) => {
    const existing = journals.find((journal) => journal.id === id);
    if (!existing) {
      return;
    }

    const updated = { ...existing, ...updates };

    try {
      await putItem(STORE_NAMES.journals, updated);
      setJournals((prev) =>
        prev.map((journal) => (journal.id === id ? updated : journal))
      );
    } catch (error) {
      console.error("Error updating journal:", error);
    }
  };

  const getJournalById = (id) => journals.find((journal) => journal.id === id);

  return {
    journals,
    addJournal,
    deleteJournal,
    updateJournal,
    getJournalById,
  };
};
```

### FILE: src\main.jsx

```text
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";
import "./styles/global.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

### FILE: src\pages\Dashboard.css

```text
/**
 * Dashboard Page Styles
 */

.dashboard-page {
  padding: var(--spacing-lg) 0;
  min-height: calc(100vh - 250px);
}

.dashboard-divider {
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--border), transparent);
  margin: var(--spacing-2xl) 0;
}

@media (max-width: 768px) {
  .dashboard-page {
    padding: var(--spacing-md) 0;
  }

  .dashboard-divider {
    margin: var(--spacing-lg) 0;
  }
}
```

### FILE: src\pages\Dashboard.jsx

```text
import MoodDashboard from "../components/Dashboard/MoodDashboard";
import JournalList from "../components/Journal/JournalList";
import "./Dashboard.css";

/**
 * Dashboard Page
 * View mood analytics and all journal entries
 */
const Dashboard = ({ journals, onDeleteJournal }) => {
  return (
    <div className="dashboard-page">
      <div className="container">
        <MoodDashboard journals={journals} />
        <div className="dashboard-divider"></div>
        <JournalList journals={journals} onDelete={onDeleteJournal} />
      </div>
    </div>
  );
};

export default Dashboard;
```

### FILE: src\pages\Expenses.css

```text
.expenses-page {
  width: 100%;
}

.expenses-header {
  margin-bottom: var(--spacing-lg);
}

.expenses-header p {
  color: var(--text-secondary);
}

.expense-stats-grid {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: var(--spacing-md);
  margin-bottom: var(--spacing-xl);
}

.expense-stat-card {
  background-color: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-sm);
  padding: var(--spacing-md);
  display: flex;
  flex-direction: column;
  gap: var(--spacing-xs);
}

.expense-stat-card span {
  color: var(--text-secondary);
  font-size: var(--font-size-sm);
}

.expense-stat-card strong {
  font-size: var(--font-size-xl);
}

@media (max-width: 1024px) {
  .expense-stats-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 768px) {
  .expense-stats-grid {
    grid-template-columns: 1fr;
  }
}
```

### FILE: src\pages\Expenses.jsx

```text
import { useMemo } from "react";
import ExpenseCharts from "../components/Expenses/ExpenseCharts";
import ExpenseForm from "../components/Expenses/ExpenseForm";
import ExpenseList from "../components/Expenses/ExpenseList";
import { useExpenses } from "../hooks/useExpenses";
import "./Expenses.css";

function Expenses() {
  const { expenses, addExpense, deleteExpense } = useExpenses();

  const stats = useMemo(() => {
    const total = expenses.reduce((sum, item) => sum + Number(item.amount), 0);

    const currentMonth = new Date();
    const monthTotal = expenses
      .filter((item) => {
        const date = new Date(item.date);
        return (
          date.getMonth() === currentMonth.getMonth() &&
          date.getFullYear() === currentMonth.getFullYear()
        );
      })
      .reduce((sum, item) => sum + Number(item.amount), 0);

    const average = expenses.length > 0 ? total / expenses.length : 0;

    return {
      total,
      monthTotal,
      average,
      entries: expenses.length,
    };
  }, [expenses]);

  return (
    <div className="expenses-page">
      <div className="container">
        <div className="expenses-header">
          <h1>ðŸ’° Daily Expenses</h1>
          <p>Track spending every day and monitor your monthly habits.</p>
        </div>

        <div className="expense-stats-grid">
          <div className="expense-stat-card">
            <span>Total Spend</span>
            <strong>â‚¹{stats.total.toFixed(2)}</strong>
          </div>
          <div className="expense-stat-card">
            <span>This Month</span>
            <strong>â‚¹{stats.monthTotal.toFixed(2)}</strong>
          </div>
          <div className="expense-stat-card">
            <span>Average Entry</span>
            <strong>â‚¹{stats.average.toFixed(2)}</strong>
          </div>
          <div className="expense-stat-card">
            <span>Entries</span>
            <strong>{stats.entries}</strong>
          </div>
        </div>

        <ExpenseForm onAddExpense={addExpense} />
        <ExpenseCharts expenses={expenses} />
        <ExpenseList expenses={expenses} onDeleteExpense={deleteExpense} />
      </div>
    </div>
  );
}

export default Expenses;
```

### FILE: src\pages\Home.css

```text
/**
 * Home Page Styles
 */

.home-page {
  width: 100%;
}

/* Hero Section */
.hero {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--spacing-2xl);
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
  padding: var(--spacing-2xl) var(--spacing-lg);
  min-height: 600px;
}

.hero-content h1 {
  font-size: 3.5rem;
  margin-bottom: var(--spacing-md);
  background: linear-gradient(
    135deg,
    var(--primary-color),
    var(--primary-light)
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.tagline {
  font-size: var(--font-size-xl);
  font-weight: 500;
  color: var(--text-secondary);
  margin-bottom: var(--spacing-md);
}

.description {
  font-size: var(--font-size-lg);
  color: var(--text-secondary);
  line-height: var(--line-height-relaxed);
  margin-bottom: var(--spacing-lg);
}

.hero-btn {
  align-self: flex-start;
  display: inline-block;
}

.hero-illustration {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

.illustration-emoji {
  font-size: 300px;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-20px);
  }
}

/* Features Section */
.features {
  background: linear-gradient(
    135deg,
    rgba(99, 102, 241, 0.05),
    rgba(129, 140, 248, 0.05)
  );
  padding: var(--spacing-2xl) var(--spacing-lg);
}

.features-container {
  max-width: 1200px;
  margin: 0 auto;
}

.features h2 {
  text-align: center;
  margin-bottom: var(--spacing-2xl);
  font-size: var(--font-size-2xl);
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: var(--spacing-lg);
}

.feature-card {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  text-align: center;
  box-shadow: var(--shadow-md);
  transition: all var(--transition-base);
  border: 1px solid var(--border);
}

.feature-card:hover {
  transform: translateY(-8px);
  box-shadow: var(--shadow-lg);
  border-color: var(--primary-color);
}

.feature-icon {
  font-size: 2.5rem;
  margin-bottom: var(--spacing-md);
}

.feature-card h3 {
  color: var(--text-primary);
  margin-bottom: var(--spacing-md);
}

.feature-card p {
  color: var(--text-secondary);
  font-size: var(--font-size-sm);
  margin: 0;
  line-height: var(--line-height-relaxed);
}

/* How It Works */
.how-it-works {
  max-width: 1200px;
  margin: 0 auto;
  padding: var(--spacing-2xl) var(--spacing-lg);
}

.how-it-works h2 {
  text-align: center;
  margin-bottom: var(--spacing-2xl);
  font-size: var(--font-size-2xl);
}

.steps {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--spacing-lg);
}

.step {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  text-align: center;
  box-shadow: var(--shadow-sm);
  border-left: 4px solid var(--primary-color);
  transition: all var(--transition-base);
}

.step:hover {
  box-shadow: var(--shadow-md);
  transform: translateY(-4px);
}

.step-number {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 50px;
  height: 50px;
  background: linear-gradient(
    135deg,
    var(--primary-color),
    var(--primary-light)
  );
  color: white;
  border-radius: 50%;
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: var(--spacing-md);
}

.step h3 {
  color: var(--text-primary);
  margin-bottom: var(--spacing-md);
}

.step p {
  color: var(--text-secondary);
  font-size: var(--font-size-sm);
  margin: 0;
  line-height: var(--line-height-relaxed);
}

/* CTA Section */
.cta {
  background: linear-gradient(
    135deg,
    var(--primary-color),
    var(--primary-light)
  );
  color: white;
  text-align: center;
  padding: var(--spacing-2xl) var(--spacing-lg);
  border-radius: var(--radius-xl);
  margin: var(--spacing-2xl) var(--spacing-lg);
}

.cta h2 {
  color: white;
  margin-bottom: var(--spacing-lg);
}

.cta .btn {
  background-color: white;
  color: var(--primary-color);
  font-weight: 600;
}

.cta .btn:hover {
  background-color: rgba(255, 255, 255, 0.9);
}

@media (max-width: 1024px) {
  .hero {
    grid-template-columns: 1fr;
    min-height: auto;
    padding: var(--spacing-xl) var(--spacing-lg);
  }

  .hero-content h1 {
    font-size: 2.5rem;
  }

  .illustration-emoji {
    font-size: 150px;
  }
}

@media (max-width: 768px) {
  .hero-content h1 {
    font-size: 2rem;
  }

  .features-grid {
    grid-template-columns: 1fr;
  }

  .steps {
    grid-template-columns: 1fr;
  }

  .cta {
    margin: var(--spacing-lg);
  }
}
```

### FILE: src\pages\Home.jsx

```text
import { Link } from "react-router-dom";
import "./Home.css";

/**
 * Home Page
 * Landing page and quick journal creation
 */
const Home = () => {
  return (
    <div className="home-page">
      <section className="hero">
        <div className="hero-content">
          <h1>ðŸ“” Daily Journal</h1>
          <p className="tagline">
            Your personal space for reflection and growth
          </p>
          <p className="description">
            Write journals, capture daily learnings, and track your expenses in
            one place. Build better self-awareness while staying on top of
            your growth and spending habits.
          </p>
          <Link to="/journals" className="btn btn-primary btn-lg hero-btn">
            âœ¨ Start Journaling
          </Link>
        </div>
        <div className="hero-illustration">
          <div className="illustration-emoji">ðŸ“</div>
        </div>
      </section>

      <section className="features">
        <div className="features-container">
          <h2>âœ¨ Features</h2>
          <div className="features-grid">
            <div className="feature-card">
              <div className="feature-icon">âœï¸</div>
              <h3>Write Freely</h3>
              <p>
                Express your thoughts and feelings with no judgment. Create as
                many entries as you want.
              </p>
            </div>

            <div className="feature-card">
              <div className="feature-icon">ðŸ’­</div>
              <h3>Track Your Mood</h3>
              <p>
                Rate your mood from 1-5 with emojis. See how your emotional
                state changes over time.
              </p>
            </div>

            <div className="feature-card">
              <div className="feature-icon">ðŸ“Š</div>
              <h3>Analyze Patterns</h3>
              <p>
                View beautiful charts and insights to understand your mood
                trends and emotional patterns.
              </p>
            </div>

            <div className="feature-card">
              <div className="feature-icon">ðŸ”</div>
              <h3>Private & Secure</h3>
              <p>
                All your entries are stored locally on your device. Your data
                stays with you, always.
              </p>
            </div>

            <div className="feature-card">
              <div className="feature-icon">ðŸ”</div>
              <h3>Search & Filter</h3>
              <p>
                Find specific entries quickly with search, or filter by mood and
                date.
              </p>
            </div>

            <div className="feature-card">
              <div className="feature-icon">ðŸ“±</div>
              <h3>Responsive Design</h3>
              <p>
                Use on any device - desktop, tablet, or mobile. Your journal
                goes wherever you do.
              </p>
            </div>

            <div className="feature-card">
              <div className="feature-icon">ðŸ’¸</div>
              <h3>Track Expenses</h3>
              <p>
                Add daily expenses, categorize spending, and review totals to
                keep better control over your money.
              </p>
            </div>

            <div className="feature-card">
              <div className="feature-icon">ðŸ“š</div>
              <h3>Log Learnings</h3>
              <p>
                Save what you learned each day with categories and references
                so your knowledge growth becomes visible.
              </p>
            </div>
          </div>
        </div>
      </section>

      <section className="how-it-works">
        <h2>ðŸš€ How It Works</h2>
        <div className="steps">
          <div className="step">
            <div className="step-number">1</div>
            <h3>Write Your Entry</h3>
            <p>
              Go to "Home" and create a new journal entry with a title, your
              thoughts, and how you're feeling.
            </p>
          </div>

          <div className="step">
            <div className="step-number">2</div>
            <h3>Choose Your Mood</h3>
            <p>
              Select your current mood from ðŸ˜¡ Terrible to ðŸ˜„ Excellent to track
              your emotional state.
            </p>
          </div>

          <div className="step">
            <div className="step-number">3</div>
            <h3>View Your Entries</h3>
            <p>
              Visit the "Journals" page to see all your entries, search, filter,
              and expand them to read more.
            </p>
          </div>

          <div className="step">
            <div className="step-number">4</div>
            <h3>Analyze Your Mood</h3>
            <p>
              Check the "Dashboard" to see beautiful charts showing your mood
              trends and emotional patterns.
            </p>
          </div>
        </div>
      </section>

      <section className="cta">
        <h2>Ready to start your journal?</h2>
        <Link to="/journals" className="btn btn-primary btn-lg">
          âœ¨ Create Your First Entry
        </Link>
      </section>
    </div>
  );
};

export default Home;
```

### FILE: src\pages\Journals.css

```text
/**
 * Journals Page Styles
 */

.journals-page {
  padding: var(--spacing-lg) 0;
  min-height: calc(100vh - 250px);
}

@media (max-width: 768px) {
  .journals-page {
    padding: var(--spacing-md) 0;
  }
}
```

### FILE: src\pages\Journals.jsx

```text
import JournalForm from "../components/Journal/JournalForm";
import "./Journals.css";

/**
 * Journals Page
 * Create and list journal entries
 */
const Journals = ({ journals, onAddJournal }) => {
  return (
    <div className="journals-page">
      <div className="container">
        <JournalForm onSubmit={onAddJournal} />
      </div>
    </div>
  );
};

export default Journals;
```

### FILE: src\pages\Learning.css

```text
/**
 * Learning Page Styles
 */

.learning-page {
  padding: var(--spacing-xl) 0;
  background-color: var(--background);
}

.page-header {
  text-align: center;
  margin-bottom: var(--spacing-2xl);
}

.page-header h1 {
  font-size: var(--font-size-3xl);
  color: var(--text-primary);
  margin-bottom: var(--spacing-md);
}

.page-subtitle {
  font-size: var(--font-size-lg);
  color: var(--text-secondary);
  margin: 0;
}

/* Stats Grid */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: var(--spacing-lg);
  margin-bottom: var(--spacing-2xl);
}

.stat-card {
  background: linear-gradient(135deg, var(--surface), var(--surface-alt));
  border-radius: var(--radius-lg);
  padding: var(--spacing-lg);
  display: flex;
  align-items: center;
  gap: var(--spacing-md);
  box-shadow: var(--shadow-sm);
  transition: transform var(--transition-fast),
    box-shadow var(--transition-fast);
  border: 1px solid var(--border);
}

.stat-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
}

.stat-icon {
  font-size: 2rem;
  line-height: 1;
}

.stat-content {
  flex: 1;
}

.stat-value {
  font-size: var(--font-size-2xl);
  font-weight: 700;
  color: var(--primary-color);
  line-height: 1;
}

.stat-label {
  font-size: var(--font-size-sm);
  color: var(--text-secondary);
  margin-top: var(--spacing-xs);
}

/* Categories Section */
.categories-section {
  background-color: var(--surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  margin-bottom: var(--spacing-2xl);
  box-shadow: var(--shadow-sm);
}

.categories-section h2 {
  font-size: var(--font-size-xl);
  color: var(--text-primary);
  margin-bottom: var(--spacing-lg);
}

.categories-list {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-md);
}

.category-item {
  display: grid;
  grid-template-columns: 120px 1fr 40px;
  align-items: center;
  gap: var(--spacing-md);
}

.category-name {
  font-weight: 600;
  color: var(--text-primary);
  font-size: var(--font-size-sm);
}

.category-bar {
  height: 24px;
  background-color: var(--surface-alt);
  border-radius: var(--radius-md);
  overflow: hidden;
  border: 1px solid var(--border);
}

.category-fill {
  height: 100%;
  background: linear-gradient(
    90deg,
    var(--primary-color),
    var(--primary-light)
  );
  transition: width var(--transition-base);
}

.category-count {
  font-weight: 700;
  color: var(--primary-color);
  text-align: right;
  font-size: var(--font-size-sm);
}

/* Learning Content */
.learning-content {
  display: grid;
  grid-template-columns: 1fr 1.2fr;
  gap: var(--spacing-xl);
}

.learning-form-section {
  position: sticky;
  top: 100px;
  height: fit-content;
}

.learning-list-section h2 {
  font-size: var(--font-size-xl);
  color: var(--text-primary);
  margin-bottom: var(--spacing-lg);
}

/* Responsive Design */
@media (max-width: 1200px) {
  .learning-content {
    grid-template-columns: 1fr;
  }

  .learning-form-section {
    position: static;
  }

  .category-item {
    grid-template-columns: 100px 1fr 40px;
  }
}

@media (max-width: 768px) {
  .page-header h1 {
    font-size: var(--font-size-2xl);
  }

  .stats-grid {
    grid-template-columns: 1fr 1fr;
    gap: var(--spacing-md);
  }

  .stat-card {
    padding: var(--spacing-md);
  }

  .stat-icon {
    font-size: 1.5rem;
  }

  .stat-value {
    font-size: var(--font-size-xl);
  }

  .stat-label {
    font-size: var(--font-size-xs);
  }

  .categories-section {
    padding: var(--spacing-lg);
  }

  .category-item {
    grid-template-columns: 90px 1fr 30px;
    gap: var(--spacing-sm);
  }

  .category-name {
    font-size: var(--font-size-xs);
  }

  .category-count {
    font-size: var(--font-size-xs);
  }
}

@media (max-width: 480px) {
  .stats-grid {
    grid-template-columns: 1fr;
  }

  .stat-card {
    gap: var(--spacing-md);
  }

  .category-item {
    grid-template-columns: 80px 1fr 25px;
  }
}
```

### FILE: src\pages\Learning.jsx

```text
import { useMemo } from "react";
import { useLearnings } from "../hooks/useLearnings";
import LearningForm from "../components/Learning/LearningForm";
import LearningList from "../components/Learning/LearningList";
import "./Learning.css";

/**
 * Learning Page
 * Displays learning entries with analytics and progress tracking
 */
function Learning() {
  const { learnings, addLearning, deleteLearning, updateLearning } =
    useLearnings();

  // Calculate statistics
  const stats = useMemo(() => {
    const totalEntries = learnings.length;
    const thisWeek = learnings.filter((item) => {
      const date = new Date(item.createdAt);
      const weekAgo = new Date();
      weekAgo.setDate(weekAgo.getDate() - 7);
      return date >= weekAgo;
    }).length;

    const thisMonth = learnings.filter((item) => {
      const date = new Date(item.createdAt);
      const now = new Date();
      return (
        date.getMonth() === now.getMonth() &&
        date.getFullYear() === now.getFullYear()
      );
    }).length;

    const categories = {};
    learnings.forEach((item) => {
      categories[item.category] = (categories[item.category] || 0) + 1;
    });

    const entriesWithLinks = learnings.filter((item) => item.link).length;

    return {
      totalEntries,
      thisWeek,
      thisMonth,
      categories,
      entriesWithLinks,
      linkPercentage:
        totalEntries > 0
          ? Math.round((entriesWithLinks / totalEntries) * 100)
          : 0,
    };
  }, [learnings]);

  const topCategories = useMemo(() => {
    return Object.entries(stats.categories)
      .sort((a, b) => b[1] - a[1])
      .slice(0, 3);
  }, [stats.categories]);

  const streak = useMemo(() => {
    if (learnings.length === 0) return 0;

    let streak = 0;
    const today = new Date();
    today.setHours(0, 0, 0, 0);

    for (let i = 0; i < 365; i++) {
      const checkDate = new Date(today);
      checkDate.setDate(checkDate.getDate() - i);

      const hasEntry = learnings.some((item) => {
        const itemDate = new Date(item.createdAt);
        itemDate.setHours(0, 0, 0, 0);
        return itemDate.getTime() === checkDate.getTime();
      });

      if (hasEntry) {
        streak++;
      } else if (i > 0) {
        break;
      }
    }

    return streak;
  }, [learnings]);

  return (
    <div className="learning-page">
      <div className="container">
        {/* Header Section */}
        <div className="page-header">
          <h1>ðŸ“š Learning Journey</h1>
          <p className="page-subtitle">
            Track your learning progress and grow your knowledge every day
          </p>
        </div>

        {/* Stats Cards */}
        <div className="stats-grid">
          <div className="stat-card">
            <div className="stat-icon">ðŸ“–</div>
            <div className="stat-content">
              <div className="stat-value">{stats.totalEntries}</div>
              <div className="stat-label">Total Entries</div>
            </div>
          </div>

          <div className="stat-card">
            <div className="stat-icon">ðŸ”¥</div>
            <div className="stat-content">
              <div className="stat-value">{streak}</div>
              <div className="stat-label">Day Streak</div>
            </div>
          </div>

          <div className="stat-card">
            <div className="stat-icon">ðŸŽ¯</div>
            <div className="stat-content">
              <div className="stat-value">{stats.thisWeek}</div>
              <div className="stat-label">This Week</div>
            </div>
          </div>

          <div className="stat-card">
            <div className="stat-icon">ðŸ“…</div>
            <div className="stat-content">
              <div className="stat-value">{stats.thisMonth}</div>
              <div className="stat-label">This Month</div>
            </div>
          </div>

          <div className="stat-card">
            <div className="stat-icon">ðŸ”—</div>
            <div className="stat-content">
              <div className="stat-value">{stats.entriesWithLinks}</div>
              <div className="stat-label">Shared Resources</div>
            </div>
          </div>

          <div className="stat-card">
            <div className="stat-icon">â­</div>
            <div className="stat-content">
              <div className="stat-value">{stats.linkPercentage}%</div>
              <div className="stat-label">With Links</div>
            </div>
          </div>
        </div>

        {/* Categories Distribution */}
        {topCategories.length > 0 && (
          <div className="categories-section">
            <h2>Top Learning Categories</h2>
            <div className="categories-list">
              {topCategories.map(([category, count]) => (
                <div key={category} className="category-item">
                  <span className="category-name">{category}</span>
                  <div className="category-bar">
                    <div
                      className="category-fill"
                      style={{
                        width: `${(count / stats.totalEntries) * 100}%`,
                      }}
                    />
                  </div>
                  <span className="category-count">{count}</span>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* Form and List */}
        <div className="learning-content">
          <div className="learning-form-section">
            <LearningForm onAddLearning={addLearning} />
          </div>

          <div className="learning-list-section">
            <h2>Your Learning Entries</h2>
            <LearningList
              learnings={learnings}
              onDelete={deleteLearning}
              onUpdate={updateLearning}
            />
          </div>
        </div>
      </div>
    </div>
  );
}

export default Learning;
```

### FILE: src\styles\global.css

```text
/**
 * Global Styles
 */

@import "./variables.css";

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: var(--font-family);
  font-size: var(--font-size-base);
  line-height: var(--line-height-normal);
  color: var(--text-primary);
  background-color: var(--background);
  transition: background-color var(--transition-base);
}

/* Typography */
h1 {
  font-size: var(--font-size-3xl);
  font-weight: 700;
  line-height: var(--line-height-tight);
  margin-bottom: var(--spacing-md);
}

h2 {
  font-size: var(--font-size-2xl);
  font-weight: 600;
  line-height: var(--line-height-tight);
  margin-bottom: var(--spacing-md);
}

h3 {
  font-size: var(--font-size-xl);
  font-weight: 600;
  line-height: var(--line-height-tight);
  margin-bottom: var(--spacing-sm);
}

p {
  margin-bottom: var(--spacing-md);
}

/* Links */
a {
  color: var(--primary-color);
  text-decoration: none;
  transition: color var(--transition-fast);
}

a:hover {
  color: var(--primary-dark);
  text-decoration: underline;
}

/* Forms */
input,
textarea,
select {
  font-family: inherit;
  font-size: inherit;
}

input[type="text"],
input[type="email"],
input[type="date"],
input[type="number"],
textarea,
select {
  width: 100%;
  padding: var(--spacing-sm) var(--spacing-md);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  background-color: var(--surface);
  color: var(--text-primary);
  transition: border-color var(--transition-fast),
    box-shadow var(--transition-fast);
}

input[type="text"]:focus,
input[type="email"]:focus,
input[type="date"]:focus,
input[type="number"]:focus,
textarea:focus,
select:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
}

textarea {
  resize: vertical;
  min-height: 120px;
  font-family: inherit;
}

/* Buttons */
button {
  cursor: pointer;
  font-family: inherit;
  font-size: inherit;
  font-weight: 500;
  border: none;
  border-radius: var(--radius-md);
  padding: var(--spacing-sm) var(--spacing-lg);
  transition: all var(--transition-fast);
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-primary {
  background-color: var(--primary-color);
  color: white;
}

.btn-primary:hover:not(:disabled) {
  background-color: var(--primary-dark);
  transform: translateY(-1px);
  box-shadow: var(--shadow-md);
}

.btn-secondary {
  background-color: var(--surface-alt);
  color: var(--text-primary);
  border: 1px solid var(--border);
}

.btn-secondary:hover:not(:disabled) {
  background-color: var(--border);
}

.btn-danger {
  background-color: var(--danger);
  color: white;
}

.btn-danger:hover:not(:disabled) {
  background-color: #dc2626;
  transform: translateY(-1px);
  box-shadow: var(--shadow-md);
}

.btn-sm {
  padding: var(--spacing-xs) var(--spacing-md);
  font-size: var(--font-size-sm);
}

.btn-lg {
  padding: var(--spacing-md) var(--spacing-xl);
  font-size: var(--font-size-lg);
}

/* Utilities */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-md);
}

.text-center {
  text-align: center;
}

.text-muted {
  color: var(--text-secondary);
}

.mt-sm {
  margin-top: var(--spacing-sm);
}

.mt-md {
  margin-top: var(--spacing-md);
}

.mt-lg {
  margin-top: var(--spacing-lg);
}

.mb-sm {
  margin-bottom: var(--spacing-sm);
}

.mb-md {
  margin-bottom: var(--spacing-md);
}

.mb-lg {
  margin-bottom: var(--spacing-lg);
}

.gap-sm {
  gap: var(--spacing-sm);
}

.gap-md {
  gap: var(--spacing-md);
}

.gap-lg {
  gap: var(--spacing-lg);
}

/* Loading and Empty States */
.loading {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 200px;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid var(--border);
  border-top-color: var(--primary-color);
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.empty-state {
  text-align: center;
  padding: var(--spacing-2xl) var(--spacing-md);
}

.empty-state h3 {
  color: var(--text-secondary);
  margin-bottom: var(--spacing-md);
}

/* Scrollbar Styling */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background: var(--background);
}

::-webkit-scrollbar-thumb {
  background: var(--border);
  border-radius: var(--radius-full);
}

::-webkit-scrollbar-thumb:hover {
  background: var(--text-tertiary);
}

/* Animations */
@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.animate-slideIn {
  animation: slideIn var(--transition-base);
}

.animate-fadeIn {
  animation: fadeIn var(--transition-base);
}
```

### FILE: src\styles\variables.css

```text
/**
 * CSS Variables and Theme Configuration
 */

:root {
  /* Primary Colors */
  --primary-color: #6366f1;
  --primary-dark: #4f46e5;
  --primary-light: #818cf8;

  /* Mood Colors */
  --mood-1: #ef4444; /* Terrible - Red */
  --mood-2: #f97316; /* Bad - Orange */
  --mood-3: #eab308; /* Neutral - Yellow */
  --mood-4: #84cc16; /* Good - Lime */
  --mood-5: #22c55e; /* Excellent - Green */

  /* Neutral Colors */
  --background: #f8fafc;
  --surface: #ffffff;
  --surface-alt: #f1f5f9;
  --border: #e2e8f0;
  --text-primary: #1e293b;
  --text-secondary: #64748b;
  --text-tertiary: #94a3b8;

  /* Semantic Colors */
  --success: #22c55e;
  --warning: #f97316;
  --danger: #ef4444;
  --info: #3b82f6;

  /* Spacing */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  --spacing-2xl: 3rem;

  /* Border Radius */
  --radius-sm: 0.375rem;
  --radius-md: 0.5rem;
  --radius-lg: 0.75rem;
  --radius-xl: 1rem;
  --radius-full: 9999px;

  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1);

  /* Transitions */
  --transition-fast: 150ms ease-in-out;
  --transition-base: 200ms ease-in-out;
  --transition-slow: 300ms ease-in-out;

  /* Typography */
  --font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    "Helvetica Neue", Arial, sans-serif;
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  --font-size-2xl: 1.5rem;
  --font-size-3xl: 1.875rem;

  --line-height-tight: 1.25;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.625;
}

/* Dark mode variant */
@media (prefers-color-scheme: dark) {
  :root {
    --background: #0f172a;
    --surface: #1e293b;
    --surface-alt: #334155;
    --border: #475569;
    --text-primary: #f1f5f9;
    --text-secondary: #cbd5e1;
    --text-tertiary: #94a3b8;
  }
}
```

### FILE: src\utils\dateHelpers.js

```text
/**
 * Date Helper Utilities
 * Handles date formatting and manipulation
 */

/**
 * Format date to readable string
 * @param {string|Date} date - Date to format
 * @returns {string} Formatted date string
 */
export const formatDate = (date) => {
  const d = new Date(date);
  return d.toLocaleDateString("en-US", {
    year: "numeric",
    month: "long",
    day: "numeric",
  });
};

/**
 * Format date to short format (e.g., "Jan 15, 2026")
 * @param {string|Date} date - Date to format
 * @returns {string} Short formatted date
 */
export const formatDateShort = (date) => {
  const d = new Date(date);
  return d.toLocaleDateString("en-US", {
    month: "short",
    day: "numeric",
    year: "2-digit",
  });
};

/**
 * Format date with time
 * @param {string|Date} date - Date to format
 * @returns {string} Formatted date and time
 */
export const formatDateTime = (date) => {
  const d = new Date(date);
  return d.toLocaleDateString("en-US", {
    year: "numeric",
    month: "long",
    day: "numeric",
    hour: "2-digit",
    minute: "2-digit",
  });
};

/**
 * Get today's date in YYYY-MM-DD format
 * @returns {string} Today's date
 */
export const getTodayDateString = () => {
  const today = new Date();
  return today.toISOString().split("T")[0];
};

/**
 * Check if date is today
 * @param {string|Date} date - Date to check
 * @returns {boolean} True if date is today
 */
export const isToday = (date) => {
  const d = new Date(date);
  const today = new Date();
  return d.toDateString() === today.toDateString();
};

/**
 * Check if date is yesterday
 * @param {string|Date} date - Date to check
 * @returns {boolean} True if date is yesterday
 */
export const isYesterday = (date) => {
  const d = new Date(date);
  const yesterday = new Date();
  yesterday.setDate(yesterday.getDate() - 1);
  return d.toDateString() === yesterday.toDateString();
};

/**
 * Get relative date string (Today, Yesterday, etc.)
 * @param {string|Date} date - Date to format
 * @returns {string} Relative date string
 */
export const getRelativeDate = (date) => {
  if (isToday(date)) {
    return "Today";
  } else if (isYesterday(date)) {
    return "Yesterday";
  } else {
    return formatDateShort(date);
  }
};

/**
 * Calculate days between two dates
 * @param {Date} date1 - First date
 * @param {Date} date2 - Second date
 * @returns {number} Number of days between dates
 */
export const daysBetween = (date1, date2) => {
  const d1 = new Date(date1);
  const d2 = new Date(date2);
  const timeDiff = Math.abs(d2 - d1);
  return Math.ceil(timeDiff / (1000 * 60 * 60 * 24));
};
```

### FILE: src\utils\indexedDb.js

```text
const DB_NAME = "dailyJournalDB";
const DB_VERSION = 1;

const STORE_NAMES = {
  journals: "journals",
  learnings: "learnings",
  expenses: "expenses",
};

let dbPromise;

function openDatabase() {
  if (dbPromise) {
    return dbPromise;
  }

  dbPromise = new Promise((resolve, reject) => {
    const request = window.indexedDB.open(DB_NAME, DB_VERSION);

    request.onupgradeneeded = (event) => {
      const db = event.target.result;

      Object.values(STORE_NAMES).forEach((storeName) => {
        if (!db.objectStoreNames.contains(storeName)) {
          db.createObjectStore(storeName, { keyPath: "id" });
        }
      });
    };

    request.onsuccess = () => {
      resolve(request.result);
    };

    request.onerror = () => {
      reject(request.error);
    };
  });

  return dbPromise;
}

function withStore(storeName, mode, operation) {
  return openDatabase().then(
    (db) =>
      new Promise((resolve, reject) => {
        const tx = db.transaction(storeName, mode);
        const store = tx.objectStore(storeName);

        let operationResult;
        try {
          operationResult = operation(store);
        } catch (error) {
          reject(error);
          return;
        }

        tx.oncomplete = () => resolve(operationResult);
        tx.onerror = () => reject(tx.error);
        tx.onabort = () => reject(tx.error || new Error("Transaction aborted"));
      })
  );
}

function requestToPromise(request) {
  return new Promise((resolve, reject) => {
    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

export async function getAllItems(storeName) {
  return withStore(storeName, "readonly", (store) => requestToPromise(store.getAll()));
}

export async function putItem(storeName, item) {
  return withStore(storeName, "readwrite", (store) => requestToPromise(store.put(item)));
}

export async function putManyItems(storeName, items) {
  return withStore(storeName, "readwrite", (store) => {
    const writes = items.map((item) => requestToPromise(store.put(item)));
    return Promise.all(writes);
  });
}

export async function deleteItem(storeName, id) {
  return withStore(storeName, "readwrite", (store) => requestToPromise(store.delete(id)));
}

export async function hasItems(storeName) {
  const all = await getAllItems(storeName);
  return all.length > 0;
}

export { STORE_NAMES };
```

### FILE: src\utils\moodCalculator.js

```text
/**
 * Mood Calculator Utility
 * Handles mood-related calculations and conversions
 */

// Mood mapping constants
export const MOOD_MAP = {
  1: "ðŸ˜¡",
  2: "ðŸ˜•",
  3: "ðŸ˜",
  4: "ðŸ˜Š",
  5: "ðŸ˜„",
};

export const MOOD_LABELS = {
  1: "Terrible",
  2: "Bad",
  3: "Neutral",
  4: "Good",
  5: "Excellent",
};

/**
 * Get mood emoji from numerical value
 * @param {number} moodValue - Value from 1-5
 * @returns {string} Emoji representation
 */
export const getMoodEmoji = (moodValue) => {
  return MOOD_MAP[moodValue] || "ðŸ˜";
};

/**
 * Get mood label from numerical value
 * @param {number} moodValue - Value from 1-5
 * @returns {string} Mood label
 */
export const getMoodLabel = (moodValue) => {
  return MOOD_LABELS[moodValue] || "Neutral";
};

/**
 * Calculate average mood from array of journals
 * @param {Array} journals - Array of journal entries
 * @returns {number} Average mood score (rounded to 1 decimal)
 */
export const calculateAverageMood = (journals) => {
  if (!journals || journals.length === 0) return 0;

  const sum = journals.reduce((acc, journal) => acc + journal.mood, 0);
  return parseFloat((sum / journals.length).toFixed(1));
};

/**
 * Calculate mood statistics for a period
 * @param {Array} journals - Array of journal entries
 * @returns {Object} Statistics object
 */
export const calculateMoodStats = (journals) => {
  if (!journals || journals.length === 0) {
    return {
      average: 0,
      highest: 0,
      lowest: 0,
      total: 0,
    };
  }

  const moods = journals.map((j) => j.mood);
  const average = calculateAverageMood(journals);
  const highest = Math.max(...moods);
  const lowest = Math.min(...moods);

  return {
    average,
    highest,
    lowest,
    total: journals.length,
  };
};

/**
 * Group journals by week
 * @param {Array} journals - Array of journal entries
 * @returns {Object} Grouped journals by week
 */
export const groupJournalsByWeek = (journals) => {
  if (!journals || journals.length === 0) return {};

  const grouped = {};

  journals.forEach((journal) => {
    const date = new Date(journal.date);
    const weekStart = getWeekStart(date);
    const weekKey = weekStart.toISOString().split("T")[0];

    if (!grouped[weekKey]) {
      grouped[weekKey] = [];
    }
    grouped[weekKey].push(journal);
  });

  return grouped;
};

/**
 * Get the start date of the week (Monday)
 * @param {Date} date - Date to get week start from
 * @returns {Date} Start of week date
 */
export const getWeekStart = (date) => {
  const d = new Date(date);
  const day = d.getDay();
  const diff = d.getDate() - day + (day === 0 ? -6 : 1);
  return new Date(d.setDate(diff));
};

/**
 * Get weekly mood data for chart display
 * @param {Array} journals - Array of journal entries
 * @returns {Array} Array of weekly mood data
 */
export const getWeeklyMoodData = (journals) => {
  const grouped = groupJournalsByWeek(journals);

  return Object.entries(grouped)
    .map(([weekStart, weekJournals]) => ({
      week: formatWeekLabel(new Date(weekStart)),
      date: weekStart,
      mood: calculateAverageMood(weekJournals),
      entries: weekJournals.length,
    }))
    .sort((a, b) => new Date(a.date) - new Date(b.date));
};

/**
 * Format week label for display
 * @param {Date} date - Date in the week
 * @returns {string} Formatted week label
 */
export const formatWeekLabel = (date) => {
  const start = getWeekStart(date);
  const end = new Date(start);
  end.setDate(end.getDate() + 6);

  return `${start.toLocaleDateString("en-US", {
    month: "short",
    day: "numeric",
  })} - ${end.toLocaleDateString("en-US", { month: "short", day: "numeric" })}`;
};

/**
 * Get last 7 days worth of data
 * @param {Array} journals - Array of journal entries
 * @returns {Array} Journals from last 7 days
 */
export const getLast7DaysJournals = (journals) => {
  const today = new Date();
  const sevenDaysAgo = new Date(today.getTime() - 7 * 24 * 60 * 60 * 1000);

  return journals.filter((journal) => {
    const journalDate = new Date(journal.date);
    return journalDate >= sevenDaysAgo && journalDate <= today;
  });
};
```

### FILE: tsconfig.json

```text
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

### FILE: tsconfig.node.json

```text
{
  "compilerOptions": {
    "composite": true,
    "skipLibCheck": true,
    "module": "ESNext",
    "moduleResolution": "bundler",
    "allowSyntheticDefaultImports": true
  },
  "include": ["vite.config.js"]
}
```

### FILE: vite.config.js

```text
import react from "@vitejs/plugin-react";
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [react()],
  server: {
    port: 5173,
    strictPort: true, // Will fail if port is in use instead of trying another
    open: true,
  },
});
```


