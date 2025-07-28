# 🌐 Geronome Web Interface

> *"Мы учим машины понимать старение, чтобы человечество могло его остановить"*

Интерактивное веб-приложение для исследования данных о долголетии через ИИ-чат, визуализацию графов знаний и анализ научных отчетов.

## 🚀 Быстрый старт

```bash
# Установка зависимостей
pnpm install

# Запуск в режиме разработки
pnpm dev

# Сборка для продакшена
pnpm build && pnpm start
```

Откройте [http://localhost:3000](http://localhost:3000) для просмотра приложения.

## 🏗️ Архитектура приложения

### 📁 Структура проекта

```
web/
├── app/                     # Next.js App Router
│   ├── page.tsx            # 🏠 Главная страница
│   ├── layout.tsx          # 📐 Общий layout
│   ├── globals.css         # 🎨 Глобальные стили
│   ├── chat/               # 💬 Чат с ИИ
│   │   └── page.tsx
│   ├── graph/              # 📊 Визуализация графов
│   │   └── page.tsx
│   ├── report/             # 📋 Исследовательские отчеты
│   │   └── page.tsx
│   ├── components/         # 🧩 Переиспользуемые компоненты
│   │   ├── ContextSelector.tsx
│   │   └── GraphVisualization.tsx
│   └── api/               # 🔌 API endpoints
│       ├── chat/          # Обработка чат-запросов
│       ├── datasets/      # Управление датасетами
│       └── graph/         # Данные для графов
├── public/                # 📁 Статические файлы
│   └── data/             # Датасеты и файлы данных
└── package.json          # 📦 Зависимости и скрипты
```

## 🎯 Основные функции

### 🏠 **Главная страница** (`/`)
- Презентация миссии Geronome
- Навигация по всем разделам приложения
- Анимированный интерфейс в стиле "matrix"

### 💬 **ИИ-Чат** (`/chat`)
- Интерактивные беседы с ИИ о долголетии
- Контекстно-зависимые ответы на основе научных данных
- Поддержка markdown форматирования

### 📊 **Граф знаний** (`/graph`)
- Интерактивная визуализация научных связей
- D3.js-powered граф с зумом и фильтрацией
- Исследование связей между:
  - Научными статьями
  - Биологическими сущностями  
  - Исследовательскими гипотезами
  - Экспериментальными методами

### 📋 **Исследовательские отчеты** (`/report`)
- Структурированные отчеты о направлениях исследований
- Приоритизированные списки перспективных идей
- Детализация по критериям: новизна, влияние, выполнимость

## 🛠️ Технический стек

### **Frontend Framework**
- **Next.js 15** - React framework с App Router
- **React 19** - UI библиотека
- **TypeScript** - типизированный JavaScript

### **Styling & UI**
- **TailwindCSS 4** - utility-first CSS framework
- **Анимации** - кастомные CSS анимации
- **Адаптивный дизайн** - оптимизация для всех устройств

### **Data Visualization**
- **D3.js 7** - мощная библиотека визуализации данных
- **Интерактивные графы** - зум, перетаскивание, фильтрация
- **SVG рендеринг** - векторная графика высокого качества

### **AI Integration**
- **Google GenAI** - интеграция с Gemini API
- **Structured Outputs** - типизированные ответы ИИ
- **Context Management** - управление контекстом беседы

### **Data Processing**
- **XML2JS** - парсинг GraphML файлов
- **React Markdown** - рендеринг markdown контента
- **Dynamic imports** - ленивая загрузка данных

## 🔌 API Endpoints

### `/api/chat`
**POST** - Обработка чат-сообщений
```typescript
interface ChatRequest {
  message: string;
  context?: string;
}

interface ChatResponse {
  response: string;
  timestamp: string;
}
```

### `/api/datasets`
**GET** - Получение списка доступных датасетов
```typescript
interface Dataset {
  name: string;
  description: string;
  files: string[];
}
```

### `/api/graph`
**GET** - Получение данных графа знаний
```typescript
interface GraphData {
  nodes: GraphNode[];
  edges: GraphEdge[];
}
```

## 📊 Визуализация данных

### **Компонент GraphVisualization**
- **Force-directed layout** - автоматическое позиционирование узлов
- **Иерархическая структура** - группировка по типам
- **Интерактивность**:
  - Зум и панорамирование
  - Hover эффекты
  - Клик для детальной информации
  - Фильтрация по типам узлов

### **Поддерживаемые форматы**
- **GraphML** - для сложных научных графов
- **JSON** - для структурированных данных
- **Markdown** - для текстовых отчетов

## 🎨 Дизайн-система

### **Цветовая палитра**
```css
--primary: #00ff41      /* Matrix green */
--secondary: #008f11    /* Dark green */  
--background: #000000   /* Pure black */
--text: #00ff41         /* Green text */
--accent: #33ff66       /* Light green */
```

### **Типографика**
- **Monospace шрифты** - консольный стиль
- **Pixel-perfect text** - четкое отображение
- **Анимированный текст** - эффекты печатания

### **Анимации**
- **Pulse effects** - пульсирующие элементы
- **Particle system** - анимированный фон
- **Smooth transitions** - плавные переходы

## 🚀 Производительность

### **Оптимизации Next.js**
- **Static Generation** - предрендеринг страниц
- **Image Optimization** - автоматическая оптимизация изображений
- **Code Splitting** - ленивая загрузка компонентов
- **Turbopack** - ускоренная сборка в dev режиме

### **Загрузка данных**
- **Инкрементальная загрузка** - большие графы по частям
- **Кэширование** - сохранение обработанных данных
- **Прогрессивное улучшение** - graceful degradation

## 🔧 Настройка окружения

### **Переменные окружения**
```bash
# Google API для ИИ функций
GOOGLE_API_KEY=your_gemini_api_key

# Настройки разработки
NODE_ENV=development
NEXT_TELEMETRY_DISABLED=1
```

### **Конфигурация TypeScript**
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["dom", "dom.iterable", "ES2022"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "incremental": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve"
  }
}
```

## 📱 Адаптивность

### **Breakpoints**
- **Mobile**: 320px - 768px
- **Tablet**: 768px - 1024px  
- **Desktop**: 1024px+

### **Mobile Optimizations**
- Touch-friendly интерфейс
- Оптимизированные анимации
- Адаптивная типографика
- Свайп-жесты для навигации

## 🚀 Деплой

### **Vercel (рекомендуется)**
```bash
# Подключение к Vercel
pnpm dlx vercel

# Автоматический деплой при git push
git push origin main
```

### **Docker**
```dockerfile
FROM node:18-alpine AS base
WORKDIR /app
COPY package*.json pnpm-lock.yaml ./
RUN npm install -g pnpm && pnpm install --frozen-lockfile
COPY . .
RUN pnpm build
EXPOSE 3000
CMD ["pnpm", "start"]
```

## 🧪 Разработка

### **Полезные команды**
```bash
# Разработка с hot reload
pnpm dev

# Проверка линтинга
pnpm lint

# Анализ бандла
pnpm build --analyze

# Очистка кэша
pnpm clean
```

### **Структура компонентов**
```typescript
// Пример компонента с TypeScript
interface ComponentProps {
  data: GraphData;
  onNodeClick: (nodeId: string) => void;
}

export default function MyComponent({ data, onNodeClick }: ComponentProps) {
  // Компонент код
}
```

## 🎯 Roadmap

### **Ближайшие планы**
- [ ] Поддержка real-time обновлений графов
- [ ] Экспорт визуализаций в PNG/SVG
- [ ] Продвинутые фильтры для графов
- [ ] Мобильное приложение

### **Долгосрочные цели**
- [ ] VR/AR визуализация 3D графов
- [ ] Collaborative editing
- [ ] Интеграция с внешними базами данных
- [ ] Machine learning insights

---

*Создано для продвижения исследований долголетия через интерактивные ИИ-технологии* 🧬
