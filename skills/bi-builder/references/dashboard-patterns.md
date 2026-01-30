# Dashboard Layout Patterns

## Table of Contents

1. [Page Structure](#page-structure)
2. [Responsive Grid Layout](#responsive-grid-layout)
3. [KPI Card Component](#kpi-card-component)
4. [Chart Card Container](#chart-card-container)
5. [Filter Bar](#filter-bar)
6. [Complete Page Example](#complete-page-example)

---

## Page Structure

```
app/
├── dashboard/
│   ├── page.tsx           # Dashboard main page
│   ├── loading.tsx        # Loading state
│   └── components/
│       ├── kpi-cards.tsx
│       ├── revenue-chart.tsx
│       ├── filters.tsx
│       └── export-button.tsx
├── api/
│   └── dashboard/
│       └── route.ts       # Data API
```

---

## Responsive Grid Layout

Use Tailwind CSS Grid for responsive layouts:

```tsx
// Equal 4-column grid (KPI cards)
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
  {/* KPI cards */}
</div>

// 2-column layout (large chart left, small chart right)
<div className="grid grid-cols-1 lg:grid-cols-3 gap-4">
  <div className="lg:col-span-2">
    {/* Large chart */}
  </div>
  <div>
    {/* Small chart or list */}
  </div>
</div>

// Equal 2-column grid
<div className="grid grid-cols-1 md:grid-cols-2 gap-4">
  {/* Charts */}
</div>

// Full-width section
<div className="w-full">
  {/* Full-width chart or table */}
</div>
```

**Complete Page Layout Template**:

```tsx
export default function DashboardPage() {
  return (
    <div className="min-h-screen bg-background">
      {/* Top navigation */}
      <header className="border-b">
        <div className="container mx-auto px-4 py-4 flex items-center justify-between">
          <h1 className="text-2xl font-bold">Dashboard</h1>
          <div className="flex items-center gap-4">
            {/* Filters and action buttons */}
          </div>
        </div>
      </header>

      {/* Main content area */}
      <main className="container mx-auto px-4 py-6 space-y-6">
        {/* KPI cards row */}
        <section>
          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
            {/* KPI cards */}
          </div>
        </section>

        {/* Main chart area */}
        <section>
          <div className="grid grid-cols-1 lg:grid-cols-3 gap-4">
            <div className="lg:col-span-2">
              {/* Main trend chart */}
            </div>
            <div>
              {/* Distribution or ranking */}
            </div>
          </div>
        </section>

        {/* Detailed analysis area */}
        <section>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            {/* Comparison charts */}
          </div>
        </section>

        {/* Data table */}
        <section>
          {/* DataTable */}
        </section>
      </main>
    </div>
  );
}
```

---

## KPI Card Component

```tsx
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { ArrowUpIcon, ArrowDownIcon } from "lucide-react";
import { cn } from "@/lib/utils";

interface KPICardProps {
  title: string;
  value: string | number;
  change?: number;
  changeLabel?: string;
  icon?: React.ReactNode;
}

export function KPICard({ title, value, change, changeLabel, icon }: KPICardProps) {
  const isPositive = change && change > 0;
  const isNegative = change && change < 0;

  return (
    <Card>
      <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
        <CardTitle className="text-sm font-medium text-muted-foreground">
          {title}
        </CardTitle>
        {icon && <div className="text-muted-foreground">{icon}</div>}
      </CardHeader>
      <CardContent>
        <div className="text-2xl font-bold">{value}</div>
        {change !== undefined && (
          <p className={cn(
            "text-xs flex items-center gap-1 mt-1",
            isPositive && "text-green-600",
            isNegative && "text-red-600",
            !isPositive && !isNegative && "text-muted-foreground"
          )}>
            {isPositive && <ArrowUpIcon className="h-3 w-3" />}
            {isNegative && <ArrowDownIcon className="h-3 w-3" />}
            <span>{Math.abs(change)}%</span>
            {changeLabel && <span className="text-muted-foreground">{changeLabel}</span>}
          </p>
        )}
      </CardContent>
    </Card>
  );
}
```

**Usage Example**:

```tsx
import { DollarSign, Users, ShoppingCart, TrendingUp } from "lucide-react";

<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
  <KPICard
    title="Total Revenue"
    value="$1,234,567"
    change={12.5}
    changeLabel="vs last month"
    icon={<DollarSign className="h-4 w-4" />}
  />
  <KPICard
    title="Active Users"
    value="23,456"
    change={8.2}
    changeLabel="vs last month"
    icon={<Users className="h-4 w-4" />}
  />
  <KPICard
    title="Orders"
    value="1,234"
    change={-3.1}
    changeLabel="vs last month"
    icon={<ShoppingCart className="h-4 w-4" />}
  />
  <KPICard
    title="Conversion Rate"
    value="3.2%"
    change={0.5}
    changeLabel="vs last month"
    icon={<TrendingUp className="h-4 w-4" />}
  />
</div>
```

---

## Chart Card Container

```tsx
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card";

interface ChartCardProps {
  title: string;
  description?: string;
  children: React.ReactNode;
  actions?: React.ReactNode;
  className?: string;
}

export function ChartCard({ title, description, children, actions, className }: ChartCardProps) {
  return (
    <Card className={className}>
      <CardHeader className="flex flex-row items-center justify-between">
        <div>
          <CardTitle>{title}</CardTitle>
          {description && <CardDescription>{description}</CardDescription>}
        </div>
        {actions && <div className="flex items-center gap-2">{actions}</div>}
      </CardHeader>
      <CardContent>
        {children}
      </CardContent>
    </Card>
  );
}
```

**Usage Example**:

```tsx
<ChartCard
  title="Revenue Trend"
  description="Revenue changes over the last 12 months"
  actions={<ExportButton data={data} filename="revenue" />}
>
  <ResponsiveContainer width="100%" height={300}>
    <LineChart data={data}>
      {/* ... */}
    </LineChart>
  </ResponsiveContainer>
</ChartCard>
```

---

## Filter Bar

```tsx
"use client";

import { useState } from "react";
import { Button } from "@/components/ui/button";
import { Calendar } from "@/components/ui/calendar";
import { Popover, PopoverContent, PopoverTrigger } from "@/components/ui/popover";
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select";
import { CalendarIcon, FilterIcon } from "lucide-react";
import { format } from "date-fns";
import { enUS } from "date-fns/locale";
import { DateRange } from "react-day-picker";

interface FiltersProps {
  onFilterChange: (filters: FilterState) => void;
}

interface FilterState {
  dateRange: DateRange | undefined;
  category: string;
  status: string;
}

export function DashboardFilters({ onFilterChange }: FiltersProps) {
  const [dateRange, setDateRange] = useState<DateRange | undefined>();
  const [category, setCategory] = useState<string>("all");
  const [status, setStatus] = useState<string>("all");

  const handleFilterChange = () => {
    onFilterChange({ dateRange, category, status });
  };

  return (
    <div className="flex flex-wrap items-center gap-4">
      {/* Date range picker */}
      <Popover>
        <PopoverTrigger asChild>
          <Button variant="outline" className="w-[280px] justify-start text-left font-normal">
            <CalendarIcon className="mr-2 h-4 w-4" />
            {dateRange?.from ? (
              dateRange.to ? (
                <>
                  {format(dateRange.from, "yyyy/MM/dd", { locale: enUS })} -{" "}
                  {format(dateRange.to, "yyyy/MM/dd", { locale: enUS })}
                </>
              ) : (
                format(dateRange.from, "yyyy/MM/dd", { locale: enUS })
              )
            ) : (
              <span>Select date range</span>
            )}
          </Button>
        </PopoverTrigger>
        <PopoverContent className="w-auto p-0" align="start">
          <Calendar
            initialFocus
            mode="range"
            defaultMonth={dateRange?.from}
            selected={dateRange}
            onSelect={setDateRange}
            numberOfMonths={2}
            locale={enUS}
          />
        </PopoverContent>
      </Popover>

      {/* Category selector */}
      <Select value={category} onValueChange={setCategory}>
        <SelectTrigger className="w-[180px]">
          <SelectValue placeholder="Select category" />
        </SelectTrigger>
        <SelectContent>
          <SelectItem value="all">All Categories</SelectItem>
          <SelectItem value="electronics">Electronics</SelectItem>
          <SelectItem value="clothing">Clothing</SelectItem>
          <SelectItem value="food">Food</SelectItem>
        </SelectContent>
      </Select>

      {/* Status selector */}
      <Select value={status} onValueChange={setStatus}>
        <SelectTrigger className="w-[150px]">
          <SelectValue placeholder="Select status" />
        </SelectTrigger>
        <SelectContent>
          <SelectItem value="all">All Status</SelectItem>
          <SelectItem value="active">Active</SelectItem>
          <SelectItem value="inactive">Inactive</SelectItem>
        </SelectContent>
      </Select>

      {/* Apply filters button */}
      <Button onClick={handleFilterChange}>
        <FilterIcon className="mr-2 h-4 w-4" />
        Apply Filters
      </Button>
    </div>
  );
}
```

---

## Complete Page Example

```tsx
// app/dashboard/page.tsx
import { Suspense } from "react";
import { KPICards } from "./components/kpi-cards";
import { RevenueChart } from "./components/revenue-chart";
import { CategoryPieChart } from "./components/category-pie";
import { TopProductsTable } from "./components/top-products";
import { DashboardFilters } from "./components/filters";
import { ExportButton } from "./components/export-button";
import { Skeleton } from "@/components/ui/skeleton";

export default function DashboardPage() {
  return (
    <div className="min-h-screen bg-background">
      <header className="border-b sticky top-0 bg-background z-10">
        <div className="container mx-auto px-4 py-4">
          <div className="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
            <h1 className="text-2xl font-bold">Sales Dashboard</h1>
            <div className="flex items-center gap-2">
              <DashboardFilters />
              <ExportButton />
            </div>
          </div>
        </div>
      </header>

      <main className="container mx-auto px-4 py-6 space-y-6">
        {/* KPI cards */}
        <Suspense fallback={<KPICardsSkeleton />}>
          <KPICards />
        </Suspense>

        {/* Main chart area */}
        <div className="grid grid-cols-1 lg:grid-cols-3 gap-4">
          <div className="lg:col-span-2">
            <Suspense fallback={<ChartSkeleton />}>
              <RevenueChart />
            </Suspense>
          </div>
          <div>
            <Suspense fallback={<ChartSkeleton />}>
              <CategoryPieChart />
            </Suspense>
          </div>
        </div>

        {/* Detailed data table */}
        <Suspense fallback={<TableSkeleton />}>
          <TopProductsTable />
        </Suspense>
      </main>
    </div>
  );
}

function KPICardsSkeleton() {
  return (
    <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
      {Array.from({ length: 4 }).map((_, i) => (
        <Skeleton key={i} className="h-[120px]" />
      ))}
    </div>
  );
}

function ChartSkeleton() {
  return <Skeleton className="h-[400px]" />;
}

function TableSkeleton() {
  return <Skeleton className="h-[300px]" />;
}
```
