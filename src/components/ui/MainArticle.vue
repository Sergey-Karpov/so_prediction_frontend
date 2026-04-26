<template>
  <div class="model-info">
    <div class="model-info__container">
      <!-- Hero секция -->
      <div class="model-info__hero">
        <h1 class="model-info__title">🚀 Sales Prediction Service</h1>
        <p class="model-info__subtitle">
          Интеллектуальная система прогнозирования розничных продаж на основе машинного обучения
        </p>
      </div>

      <!-- Основная сетка -->
      <div class="model-info__grid">
        <!-- О сервисе -->
        <div class="model-info__card">
          <div class="model-info__card-icon">📊</div>
          <h3 class="model-info__card-title">О сервисе</h3>
          <p class="model-info__card-text">
            Сервис прогнозирует средний объем продаж (<strong>avg</strong>) розничных точек на
            основе ассортиментных и рыночных данных.
          </p>
          <div class="model-info__features">
            <span class="model-info__feature-tag">🎯 R² = 0.72</span>
            <span class="model-info__feature-tag">📊 MAE = 100k ₽</span>
            <span class="model-info__feature-tag">⚡ 0.1 сек</span>
            <span class="model-info__feature-tag">🔄 API Ready</span>
          </div>
        </div>

        <!-- scikit-learn информация -->
        <div class="model-info__card">
          <div class="model-info__card-icon">🐍</div>
          <h3 class="model-info__card-title">Библиотека scikit-learn</h3>
          <p class="model-info__card-text">
            Основной фреймворк для реализации пайплайнов машинного обучения. Предоставляет
            инструменты для предобработки, валидации и построения моделей.
          </p>
          <a href="https://scikit-learn.org/stable/" target="_blank" class="model-info__link">
            📖 Официальная документация →
          </a>
        </div>

        <!-- RandomForest информация -->
        <div class="model-info__card">
          <div class="model-info__card-icon">🌲</div>
          <h3 class="model-info__card-title">Random Forest Regressor</h3>
          <p class="model-info__card-text">
            Ансамблевый алгоритм, использующий 600 деревьев решений. Устойчив к выбросам и
            переобучению, даёт интерпретируемую важность признаков.
          </p>
          <a
            href="https://towardsdatascience.com/random-forest-regression-5f605132d19d"
            target="_blank"
            class="model-info__link"
          >
            📚 Статья о применении в регрессии →
          </a>
        </div>

        <!-- Важность признаков -->
        <div class="model-info__card model-info__card--full">
          <div class="model-info__card-icon">🎯</div>
          <h3 class="model-info__card-title">Важность признаков (Feature Importance)</h3>
          <div class="model-info__table-wrapper">
            <table class="model-info__table">
              <thead>
                <tr>
                  <th>Признак</th>
                  <th>Вес влияния</th>
                  <th>Визуализация</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="feature in featureImportance" :key="feature.name">
                  <td class="model-info__table-feature">{{ feature.name }}</td>
                  <td class="model-info__table-percent">
                    {{ (feature.importance * 100).toFixed(2) }}%
                  </td>
                  <td class="model-info__table-progress">
                    <div class="model-info__progress-bar">
                      <div
                        class="model-info__progress-fill"
                        :style="{ width: `${feature.importance * 100}%` }"
                      ></div>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <p class="model-info__note">
            💡 <strong>Ключевое открытие:</strong> ассортимент молока (26%) и каш (10.6%) — главные
            драйверы продаж. Взаимодействие категорий (<em>cereals_milk_multi</em>) даёт
            дополнительный 18.6% прогнозной силы.
          </p>
        </div>

        <!-- Данные обучения -->
        <div class="model-info__card">
          <div class="model-info__card-icon">📚</div>
          <h3 class="model-info__card-title">Данные для обучения</h3>
          <ul class="model-info__list">
            <li><strong>Объём выборки:</strong> 2 091 запись</li>
            <li><strong>Период:</strong> исторические данные о продажах</li>
            <li><strong>Города:</strong> Москва, СПб, Симферополь, Орёл и др.</li>
            <li><strong>Сети:</strong> Ашан, Детский мир, Лента</li>
            <li><strong>Исходные признаки:</strong> 6 → после генерации: 15</li>
          </ul>
        </div>

        <!-- Метрики -->
        <div class="model-info__card">
          <div class="model-info__card-icon">📈</div>
          <h3 class="model-info__card-title">Метрики качества</h3>
          <div class="model-info__table-wrapper">
            <table class="model-info__metrics-table">
              <thead>
                <tr>
                  <th>Метрика</th>
                  <th>Train</th>
                  <th>Test</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>
                    <strong>MAE</strong>
                    <span class="model-info__metric-hint" title="Средняя абсолютная ошибка">ⓘ</span>
                  </td>
                  <td>73 345 ₽</td>
                  <td>100 541 ₽</td>
                </tr>
                <tr>
                  <td>
                    <strong>RMSE</strong>
                    <span class="model-info__metric-hint" title="Среднеквадратичная ошибка">ⓘ</span>
                  </td>
                  <td>110 979 ₽</td>
                  <td>151 530 ₽</td>
                </tr>
                <tr class="model-info__highlight">
                  <td>
                    <strong>R² Score</strong>
                    <span class="model-info__metric-hint" title="Коэффициент детерминации">ⓘ</span>
                  </td>
                  <td>0.8556</td>
                  <td>0.7178</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="model-info__metrics-explanation">
            <h4>🎯 Почему R² = 0.72 — хороший результат для Retail?</h4>
            <p>
              В розничной торговле прогнозирование среднего чека крайне сложно из-за: сезонности,
              промо-акций, поведения покупателей. R² > 0.7 означает, что модель объясняет
              <strong>более 70% вариативности</strong> продаж — отличный показатель для
              production-систем.
            </p>
          </div>

          <div class="model-info__links">
            <a
              href="https://en.wikipedia.org/wiki/Mean_absolute_error"
              target="_blank"
              class="model-info__link model-info__link--small"
            >
              📘 Что такое MAE?
            </a>
            <a
              href="https://en.wikipedia.org/wiki/Root_mean_square_deviation"
              target="_blank"
              class="model-info__link model-info__link--small"
            >
              📘 Что такое RMSE?
            </a>
            <a
              href="https://en.wikipedia.org/wiki/Coefficient_of_determination"
              target="_blank"
              class="model-info__link model-info__link--small"
            >
              📘 Что такое R²?
            </a>
          </div>
        </div>

        <!-- Технические детали -->
        <div class="model-info__card">
          <div class="model-info__card-icon">⚙️</div>
          <h3 class="model-info__card-title">Технические детали модели</h3>
          <ul class="model-info__list">
            <li><strong>Алгоритм:</strong> Random Forest (600 деревьев)</li>
            <li><strong>Max depth:</strong> 10</li>
            <li><strong>Max features:</strong> sqrt</li>
            <li><strong>Max samples:</strong> 65% (≈bootstrapping)</li>
            <li><strong>Обработка выбросов:</strong> IQR метод</li>
            <li><strong>Генерация признаков:</strong> автоматическая</li>
          </ul>
        </div>
      </div>

      <!-- Footer -->
      <div class="model-info__footer">
        <p>
          🧠 Модель обучена с использованием
          <a href="https://scikit-learn.org/" target="_blank">scikit-learn</a>
          и сохранена в production-ready формате (joblib/pickle)
        </p>
        <p class="model-info__version">
          Версия модели: 1.0 | Автор: Sergey Karpov | Дата создания: 2026
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

// Данные из лога обучения
const featureImportance = computed(() => [
  { name: 'milk (количество SKU молока)', importance: 0.261542 },
  { name: 'cereals_milk_multi (взаимодействие)', importance: 0.186184 },
  { name: 'cereals_milk_ratio (соотношение)', importance: 0.120852 },
  { name: 'cereals (количество SKU каш)', importance: 0.106054 },
  { name: 'market_share (доля рынка)', importance: 0.057267 },
  { name: 'detmir_count_share_in_city (доля Детмира)', importance: 0.037081 },
  { name: 'chain_Детский мир (сеть)', importance: 0.035025 },
  { name: 'lenta_count_share_in_city (доля Ленты)', importance: 0.034391 },
  { name: 'lenta_count_in_city (кол-во Лент)', importance: 0.033757 },
  { name: 'population (население)', importance: 0.032332 },
  { name: 'top_chains_stores_count (кол-во топ-сетей)', importance: 0.032003 },
  { name: 'detmir_count_in_city (кол-во Детмиров)', importance: 0.025668 },
  { name: 'chain_Лента (сеть)', importance: 0.018281 },
  { name: 'aushan_count_share_in_city (доля Ашана)', importance: 0.010653 },
  { name: 'aushan_count_in_city (кол-во Ашанов)', importance: 0.008911 },
])
</script>

<style scoped lang="scss">
.model-info {
  width: 100%;
  min-height: 100vh;
  padding: 20px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;

  @media (max-width: 1024px) {
    padding: 12px;
  }

  @media (max-width: 768px) {
    padding: 8px;
  }

  &__container {
    max-width: 1400px;
    margin: 0 auto;
  }

  &__hero {
    text-align: center;
    margin-bottom: 50px;

    @media (max-width: 1024px) {
      margin-bottom: 24px;
    }

    @media (max-width: 768px) {
      margin-bottom: 16px;
    }
  }

  &__title {
    font-size: 3rem;
    font-weight: 800;
    color: white;
    margin-bottom: 15px;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);

    @media (max-width: 768px) {
      font-size: 1.8rem;
    }

    @media (max-width: 480px) {
      font-size: 1.5rem;
    }
  }

  &__subtitle {
    font-size: 1.2rem;
    color: rgba(255, 255, 255, 0.95);
    max-width: 700px;
    margin: 0 auto;

    @media (max-width: 768px) {
      font-size: 1rem;
      padding: 0 16px;
    }
  }

  &__grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(450px, 1fr));
    gap: 25px;
    margin-bottom: 40px;

    @media (max-width: 1024px) {
      grid-template-columns: 1fr;
      margin-bottom: 32px;
    }

    @media (max-width: 768px) {
      gap: 16px;
      margin-bottom: 24px;
    }
  }

  &__card {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 16px;
    background: white;
    border-radius: 16px;
    padding: 25px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
    transition:
      transform 0.3s ease,
      box-shadow 0.3s ease;

    @media (max-width: 1024px) {
      padding: 20px;
      gap: 14px;
    }

    @media (max-width: 768px) {
      padding: 16px;
      gap: 12px;
    }

    @media (max-width: 480px) {
      padding: 12px;
      gap: 10px;
    }

    &:hover {
      transform: translateY(-5px);
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.15);
    }

    &--full {
      grid-column: 1 / -1;
    }

    &-icon {
      font-size: 2.5rem;

      @media (max-width: 768px) {
        font-size: 2rem;
      }
    }

    &-title {
      font-size: 1.5rem;
      font-weight: 700;
      color: #333;

      @media (max-width: 768px) {
        font-size: 1.3rem;
      }
    }

    &-text {
      color: #666;
      line-height: 1.6;
      font-size: 0.95rem;

      @media (max-width: 768px) {
        font-size: 0.9rem;
      }
    }
  }

  &__features {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    margin-top: 15px;

    @media (max-width: 480px) {
      gap: 8px;
    }
  }

  &__feature-tag {
    background: #f0f0f0;
    padding: 5px 12px;
    border-radius: 20px;
    font-size: 0.85rem;
    color: #555;

    @media (max-width: 480px) {
      font-size: 0.75rem;
      padding: 4px 10px;
    }
  }

  &__link {
    color: #667eea;
    text-decoration: none;
    font-weight: 500;
    display: inline-flex;
    align-items: center;
    gap: 5px;
    transition: color 0.3s;

    &:hover {
      color: #764ba2;
    }

    &--small {
      font-size: 0.85rem;

      @media (max-width: 480px) {
        font-size: 0.75rem;
      }
    }
  }

  &__table-wrapper {
    overflow-x: auto;
    margin: 20px 0;
    width: 100%;
    -webkit-overflow-scrolling: touch;

    @media (max-width: 1024px) {
      margin: 12px 0;
    }

    @media (max-width: 768px) {
      margin: 8px 0;
    }
  }

  &__table {
    width: 100%;
    min-width: 500px; // Минимальная ширина для читаемости
    border-collapse: collapse;
    font-size: 0.9rem;

    @media (max-width: 768px) {
      font-size: 0.8rem;
      min-width: 400px;
    }

    @media (max-width: 480px) {
      min-width: 320px;
      font-size: 0.7rem;
    }

    @media (max-width: 390px) {
      min-width: 270px;
      font-size: 0.4rem;
    }

    th,
    td {
      padding: 12px;
      text-align: left;
      border-bottom: 1px solid #e0e0e0;

      @media (max-width: 768px) {
        padding: 8px;
      }

      @media (max-width: 480px) {
        padding: 6px;
      }
    }

    th {
      background: #f5f5f5;
      font-weight: 600;
      color: #555;
    }

    td {
      color: #666;
    }

    &-feature {
      max-width: 200px;
      white-space: normal;
      word-break: break-word;

      @media (max-width: 768px) {
        max-width: 150px;
      }

      @media (max-width: 480px) {
        max-width: 120px;
      }
    }

    &-percent {
      white-space: nowrap;
    }

    &-progress {
      min-width: 100px;

      @media (max-width: 480px) {
        min-width: 80px;
      }
    }
  }

  &__progress-bar {
    background: #e0e0e0;
    border-radius: 10px;
    height: 20px;
    overflow: hidden;
    width: 100%;

    @media (max-width: 768px) {
      height: 16px;
    }

    @media (max-width: 480px) {
      height: 12px;
    }
  }

  &__progress-fill {
    background: linear-gradient(90deg, #667eea, #764ba2);
    height: 100%;
    border-radius: 10px;
    transition: width 0.5s ease;
  }

  &__note {
    background: #f9f9f9;
    padding: 15px;
    border-radius: 8px;
    margin-top: 15px;
    font-size: 0.9rem;
    color: #555;
    border-left: 3px solid #667eea;

    @media (max-width: 768px) {
      padding: 12px;
      font-size: 0.85rem;
    }

    @media (max-width: 480px) {
      padding: 10px;
      font-size: 0.8rem;
    }
  }

  &__list {
    list-style: none;
    padding: 0;
    margin: 0;

    li {
      padding: 8px 0;
      color: #666;
      border-bottom: 1px solid #f0f0f0;

      @media (max-width: 768px) {
        padding: 6px 0;
        font-size: 0.9rem;
      }

      strong {
        color: #333;
      }
    }
  }

  &__metrics-table {
    width: 100%;
    min-width: 300px;
    border-collapse: collapse;

    @media (max-width: 480px) {
      min-width: 280px;
    }

    th,
    td {
      padding: 10px;
      text-align: center;
      border: 1px solid #e0e0e0;
      background: #181818;

      @media (max-width: 768px) {
        padding: 8px;
        font-size: 0.85rem;
      }

      @media (max-width: 480px) {
        padding: 6px;
        font-size: 0.75rem;
      }
    }

    th {
      background: #181818;
      font-weight: 600;
    }

    .model-info__highlight {
      background: #181818;
      font-weight: bold;
    }

    .model-info__metric-hint {
      display: inline-block;
      width: 16px;
      height: 16px;
      background: #ccc;
      color: white;
      border-radius: 50%;
      font-size: 10px;
      text-align: center;
      line-height: 16px;
      cursor: help;
      margin-left: 5px;

      @media (max-width: 768px) {
        width: 14px;
        height: 14px;
        font-size: 8px;
        line-height: 14px;
      }
    }
  }

  &__metrics-explanation {
    background: linear-gradient(135deg, #f5f5f5 0%, #e8e8e8 100%);
    padding: 15px;
    border-radius: 10px;

    @media (max-width: 768px) {
      padding: 12px;
    }

    h4 {
      margin: 0 0 10px 0;
      color: #333;

      @media (max-width: 768px) {
        font-size: 1rem;
      }
    }

    p {
      margin: 0;
      color: #555;
      font-size: 0.9rem;
      line-height: 1.5;

      @media (max-width: 768px) {
        font-size: 0.85rem;
      }
    }
  }

  &__links {
    display: flex;
    gap: 15px;
    flex-wrap: wrap;

    @media (max-width: 480px) {
      gap: 10px;
    }
  }

  &__footer {
    text-align: center;
    padding: 30px;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 16px;
    color: white;
    margin-bottom: 24px;

    @media (max-width: 1024px) {
      padding: 24px;
      margin-bottom: 16px;
    }

    @media (max-width: 768px) {
      padding: 20px;
      font-size: 0.9rem;
    }

    @media (max-width: 480px) {
      padding: 16px;
      font-size: 0.8rem;
    }

    a {
      color: white;
      text-decoration: underline;
    }
  }

  &__version {
    font-size: 0.85rem;
    opacity: 0.8;
    margin-top: 10px;

    @media (max-width: 768px) {
      font-size: 0.75rem;
    }
  }
}
</style>
