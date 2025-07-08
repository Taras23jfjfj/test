<!DOCTYPE html>
<html lang="uk">
<head>
  <meta charset="UTF-8">
  <title>Управління операційними витратами</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest"></script>
</head>
<body class="min-h-screen bg-gray-50">

  <!-- Верхня панель -->
  <div class="bg-white shadow-sm">
    <div class="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
      <h1 class="text-xl font-semibold text-gray-800">Управління операційними витратами</h1>
      <div class="flex items-center space-x-4">
        <span class="text-green-600 font-medium">АСТОР</span>
        <!-- Кнопка налаштувань -->
        <button id="settings-button" class="p-2 hover:bg-gray-100 rounded">
          <i data-lucide="settings"></i>
        </button>
      </div>
    </div>
  </div>

  <!-- Вкладки -->
  <div class="max-w-7xl mx-auto px-4 py-6">
    <div class="bg-white rounded-lg shadow">
      <div class="border-b">
        <div class="flex space-x-1">
          <button id="tab-planning" class="px-6 py-3 text-sm font-medium border-b-2 border-blue-500 text-blue-600 bg-blue-50">Планування</button>
          <button id="tab-approval" class="px-6 py-3 text-sm font-medium text-gray-600 hover:text-gray-800 hover:bg-gray-50">Погодження</button>
          <button id="tab-rejections" class="px-6 py-3 text-sm font-medium text-gray-600 hover:text-gray-800 hover:bg-gray-50">Відмови</button>
        </div>
      </div>
      <div class="p-6">
        <!-- Планування -->
        <div id="view-planning">
          <div class="space-y-4">
            <div class="flex justify-between items-center">
              <div class="flex space-x-4">
                <div class="flex items-center space-x-2">
                  <input id="planning-date-start" type="date" class="border rounded px-3 py-2 text-sm" value="2025-01-01">
                  <input id="planning-date-end"   type="date" class="border rounded px-3 py-2 text-sm" value="2025-07-31">
                </div>
                <button id="btn-refresh" class="flex items-center space-x-2 px-4 py-2 border rounded hover:bg-gray-50">
                  <i data-lucide="refresh-cw" class="stroke-current text-gray-600"></i><span>Оновити</span>
                </button>
              </div>
              <div class="text-sm text-gray-600">
                Поточний відділ: <span id="current-dept" class="font-medium text-blue-600">Відділ обліку персоналу</span>
              </div>
            </div>
            <!-- Таблиця Планування -->
            <div class="overflow-x-auto">
              <table class="w-full border-collapse border border-gray-300">
                <thead>
                  <tr class="bg-gray-100">
                    <th class="border p-3 text-left text-sm font-medium">Дирекція</th>
                    <th class="border p-3 text-left text-sm font-medium">Найменування категорії</th>
                    <th class="border p-3 text-left text-sm font-medium">Уточнення витрат</th>
                    <th class="border p-3 text-left text-sm font-medium">Початок</th>
                    <th class="border p-3 text-left text-sm font-medium">Кінець</th>
                    <th class="border p-3 text-left text-sm font-medium bg-blue-50 text-blue-700">Мої витрати</th>
                    <th class="border p-3 text-left text-sm font-medium">Загальні витрати</th>
                    <th class="border p-3 text-left text-sm font-medium">Місце планування</th>
                    <th class="border p-3 text-left text-sm font-medium">Назва місця планування</th>
                  </tr>
                </thead>
                <tbody id="planning-table-body">
                  <!-- Записи генеруються скриптом -->
                </tbody>
                <tfoot>
                  <tr class="bg-blue-50 border-t-2 border-blue-200">
                    <!-- Підсумок охоплює перші 5 колонок -->
                    <td class="p-3 text-sm font-bold" colspan="5">Підсумок</td>
                    <td id="sum-my-expenses"    class="p-3 text-sm font-bold text-blue-700 bg-blue-100">0</td>
                    <td id="sum-total-expenses" class="p-3 text-sm font-bold text-blue-700">0</td>
                    <td class="p-3 text-sm" colspan="2"></td>
                  </tr>
                </tfoot>
              </table>
            </div>
            <div class="flex justify-between text-sm text-gray-600">
              <span>Записів: <span id="total-count">0</span></span>
              <span>Мої витрати: <span id="sum-my-display" class="font-medium text-blue-600">0 грн</span> | Загальні витрати: <span id="sum-total-display" class="font-medium">0 грн</span></span>
            </div>
          </div>
        </div>

        <!-- Погодження (з вкладками) -->
        <div id="view-approval" class="hidden">
          <div class="space-y-4">
            <!-- Sub-tabs -->
            <div class="flex space-x-1 border-b">
              <button id="subtab-created"   class="px-4 py-2 text-sm font-medium border-b-2 border-blue-500 text-blue-600">Створені
                <span id="count-created" class="ml-2 bg-blue-50 text-blue-600 text-xs px-2 py-0.5 rounded-full">0</span>
              </button>
              <button id="subtab-check"     class="px-4 py-2 text-sm font-medium text-gray-600 hover:text-gray-800">Перевірка
                <span id="count-check" class="ml-2 bg-blue-50 text-blue-600 text-xs px-2 py-0.5 rounded-full">0</span>
              </button>
              <button id="subtab-evaluation" class="px-4 py-2 text-sm font-medium text-gray-600 hover:text-gray-800">На оцінку
                <span id="count-evaluation" class="ml-2 bg-blue-50 text-blue-600 text-xs px-2 py-0.5 rounded-full">0</span>
              </button>
              <button id="subtab-pending"   class="px-4 py-2 text-sm font-medium text-gray-600 hover:text-gray-800">На погодженні
                <span id="count-pending" class="ml-2 bg-blue-50 text-blue-600 text-xs px-2 py-0.5 rounded-full">0</span>
              </button>
              <button id="subtab-revision"  class="px-4 py-2 text-sm font-medium text-gray-600 hover:text-gray-800">На доопрацюванні
                <span id="count-revision" class="ml-2 bg-blue-50 text-blue-600 text-xs px-2 py-0.5 rounded-full">0</span>
              </button>
            </div>

        <!-- Вкладка Створені -->
        <div id="approval-created-view"
            class="tab-content hidden"
            data-status="created">
          <!-- Кнопка створення -->
          <div class="flex justify-end mb-2">
            <button id="btn-create-expense"
                    class="flex items-center px-3 py-1 bg-green-500 text-white rounded hover:bg-green-600">
              <i data-lucide="plus"></i><span class="ml-1">Створити витрату</span>
            </button>
          </div>
          <div class="overflow-x-auto">
            <table class="w-full border-collapse border border-gray-300">
              <thead>
                <tr class="bg-gray-100">
                  <th class="border p-3 text-left text-sm font-medium">Дирекція</th>
                  <th class="border p-3 text-left text-sm font-medium">Початок</th>
                  <th class="border p-3 text-left text-sm font-medium">Кінець</th>
                  <th class="border p-3 text-left text-sm font-medium bg-blue-50 text-blue-700">Мої витрати</th>
                  <th class="border p-3 text-left text-sm font-medium">Загальні витрати</th>
                  <th class="border p-3 text-left text-sm font-medium">Місце планування</th>
                  <th class="border p-3 text-left text-sm font-medium">Назва місця планування</th>
                  <th class="border p-3 text-left text-sm font-medium">Дії</th>
                </tr>
              </thead>
              <tbody id="approval-created-body"></tbody>
            </table>
          </div>
          <div id="approval-created-empty"
              class="text-center py-8 text-gray-500 hidden">
            Не знайдено записів.
          </div>
        </div>


            <!-- Вкладка Перевірка -->
            <div id="approval-check-view" class="hidden">
              <div class="overflow-x-auto">
                <table class="w-full border-collapse border border-gray-300">
                  <thead>
                    <tr class="bg-gray-100">
                      <th class="border p-3 text-left text-sm font-medium">Дирекція</th>
                      <th class="border p-3 text-left text-sm font-medium">Початок</th>
                      <th class="border p-3 text-left text-sm font-medium">Кінець</th>
                      <th class="border p-3 text-left text-sm font-medium bg-blue-50 text-blue-700">Мої витрати</th>
                      <th class="border p-3 text-left text-sm font-medium">Загальні витрати</th>
                      <th class="border p-3 text-left text-sm font-medium">Місце планування</th>
                      <th class="border p-3 text-left text-sm font-medium">Назва місця планування</th>
                      <th class="border p-3 text-left text-sm font-medium">Дії</th>
                    </tr>
                  </thead>
                  <tbody id="approval-check-body"></tbody>
                </table>
              </div>
              <div id="approval-check-empty" class="text-center py-8 text-gray-500 hidden">Не знайдено записів.</div>
            </div>

            <!-- Вкладка На оцінку (експерт) -->
            <div id="approval-evaluation-view" class="hidden">
              <div class="overflow-x-auto">
                <table class="w-full border-collapse border border-gray-300">
                  <thead>
                    <tr class="bg-gray-100">
                      <th class="border p-3 text-left text-sm font-medium">Дирекція</th>
                      <th class="border p-3 text-left text-sm font-medium">Початок</th>
                      <th class="border p-3 text-left text-sm font-medium">Кінець</th>
                      <th class="border p-3 text-left text-sm font-medium bg-blue-50 text-blue-700">Мої витрати</th>
                      <th class="border p-3 text-left text-sm font-medium">Загальні витрати</th>
                      <th class="border p-3 text-left text-sm font-medium">Місце планування</th>
                      <th class="border p-3 text-left text-sm font-medium">Назва місця планування</th>
                      <th class="border p-3 text-left text-sm font-medium">Дії</th>
                    </tr>
                  </thead>
                  <tbody id="approval-evaluation-body"></tbody>
                </table>
              </div>
              <div id="approval-evaluation-empty" class="text-center py-8 text-gray-500 hidden">Не знайдено записів.</div>
            </div>

            <div id="approval-pending-view" class="space-y-4">
              <table class="w-full border-collapse border border-gray-300">
                <thead>
                  <tr class="bg-gray-100">
                    <th class="border p-3 text-left text-sm font-medium">Дирекція</th>
                    <th class="border p-3 text-left text-sm font-medium">Початок</th>
                    <th class="border p-3 text-left text-sm font-medium">Кінець</th>
                    <th class="border p-3 text-left text-sm font-medium bg-blue-50 text-blue-700">Мої витрати</th>
                    <th class="border p-3 text-left text-sm font-medium">Загальні витрати</th>
                    <th class="border p-3 text-left text-sm font-medium">Місце планування</th>
                    <th class="border p-3 text-left text-sm font-medium">Назва місця планування</th>
                    <th class="border p-3 text-left text-sm font-medium">Статус погодження</th>
                    <th class="border p-3 text-left text-sm font-medium">Дії</th>
                  </tr>
                </thead>
                <tbody id="approval-pending-body"></tbody>
              </table>
              <div id="approval-pending-empty" class="text-center py-8 text-gray-500 hidden">
                Не знайдено записів.
              </div>
            </div>


            <!-- Вкладка На доопрацюванні (оновлена з колонкою коментаря) -->
          <div id="approval-revision-view" class="hidden">
            <div class="overflow-x-auto">
              <table class="w-full border-collapse border border-gray-300">
                <thead>
                  <tr class="bg-gray-100">
                    <th class="border p-3 text-left text-sm font-medium">Дирекція</th>
                    <th class="border p-3 text-left text-sm font-medium">Початок</th>
                    <th class="border p-3 text-left text-sm font-medium">Кінець</th>
                    <th class="border p-3 text-left text-sm font-medium bg-blue-50 text-blue-700">Мої витрати</th>
                    <th class="border p-3 text-left text-sm font-medium">Загальні витрати</th>
                    <th class="border p-3 text-left text-sm font-medium">Місце планування</th>
                    <th class="border p-3 text-left text-sm font-medium">Назва місця планування</th>
                    <th class="border p-3 text-left text-sm font-medium">Коментар</th>
                    <th class="border p-3 text-left text-sm font-medium">Дії</th>
                  </tr>
                </thead>
                <tbody id="approval-revision-body"></tbody>
              </table>
            </div>
            <div id="approval-revision-empty" class="text-center py-8 text-gray-500 hidden">Не знайдено записів.</div>
          </div>

          <!-- Модальне вікно для відправки на доопрацювання -->
          <div id="modal-revision" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center hidden z-50">
            <div class="bg-white p-6 rounded-lg shadow-lg w-96">
              <div class="flex justify-between items-center mb-4">
                <h2 class="text-lg font-medium">На доопрацювання</h2>
                <button id="modal-revision-close" class="text-gray-500 hover:text-gray-700">
                  <i data-lucide="x" class="h-5 w-5"></i>
                </button>
              </div>
              <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700 mb-2">Причина відправки на доопрацювання</label>
                <textarea id="input-revision-comment" class="w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent" rows="4" placeholder="Вкажіть причину відправки на доопрацювання..."></textarea>
              </div>
              <div class="flex space-x-3">
                <button id="modal-revision-cancel" class="flex-1 px-4 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50">Скасувати</button>
                <button id="modal-revision-save" class="flex-1 px-4 py-2 bg-yellow-500 text-white rounded-md hover:bg-yellow-600">Відправити</button>
              </div>
            </div>
          </div>

        <!-- Відмови -->
        <div id="view-rejections" class="hidden">
          <div class="space-y-4">
            <div class="overflow-x-auto">
              <table class="w-full border-collapse border border-gray-300">
                <thead>
                  <tr class="bg-gray-100">
                    <th class="border p-3 text-left text-sm font-medium">Дирекція</th>
                    <th class="border p-3 text-left text-sm font-medium">Початок</th>
                    <th class="border p-3 text-left text-sm font-medium">Кінець</th>
                    <th class="border p-3 text-left text-sm font-medium bg-blue-50 text-blue-700">Мої витрати</th>
                    <th class="border p-3 text-left text-sm font-medium">Загальні витрати</th>
                    <th class="border p-3 text-left text-sm font-medium">Місце планування</th>
                    <th class="border p-3 text-left text-sm font-medium">Назва місця планування</th>
                    <th class="border p-3 text-left text-sm font-medium">Коментар про відмову</th>
                    <th class="border p-3 text-left text-sm font-medium">Джерело відмови</th>
                  </tr>
                </thead>
                <tbody id="rejections-body"></tbody>
              </table>
            </div>
            <div id="rejections-empty" class="text-center py-8 text-gray-500 hidden">Не знайдено записів.</div>
          </div>
        </div>

      </div>
    </div>
  </div>

  <!-- Модальне вікно: Створення витрати -->
  <div id="modal-create-expense" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
    <div class="bg-white rounded-lg shadow-xl w-full max-w-2xl mx-4">
      <div class="flex justify-between items-center p-6 border-b">
        <h2 class="text-lg font-semibold">Планування витрат за період</h2>
        <button id="modal-create-close" class="text-gray-400 hover:text-gray-600">
          <i data-lucide="x"></i>
        </button>
      </div>
      <div class="p-6 space-y-4">
        <div class="grid grid-cols-2 gap-4">
          <input id="input-start-date" type="date" class="border rounded px-3 py-2">
          <input id="input-end-date"   type="date" class="border rounded px-3 py-2">
        </div>
        <div class="space-y-2">
          <label class="block text-sm font-medium">Місце планування</label>
          <select id="input-department" class="w-full border rounded px-3 py-2 text-sm">
            <option>Підрозділ</option>
            <option>Інше</option>
          </select>
        </div>
        <div class="space-y-2">
          <label class="block text-sm font-medium">Найменування категорії</label>
          <div class="flex items-center">
            <input id="input-category" type="text" class="w-full border rounded px-3 py-2" placeholder="Найменування категорії">
            <button id="open-category-window" class="ml-2 p-2 hover:bg-gray-100 rounded">
              <i data-lucide="grid"></i>
            </button>
          </div>
        </div>
        <div class="space-y-2">
          <label class="block text-sm font-medium">Опис витрат</label>
          <textarea id="input-description" class="w-full border rounded px-3 py-2 h-24" placeholder="Введіть опис витрат..."></textarea>
        </div>
        <div class="space-y-2">
          <label class="block text-sm font-medium">Сума</label>
          <input id="input-amount" type="number" class="w-full border rounded px-3 py-2" placeholder="0">
        </div>
        <div class="space-y-2">
          <label class="block text-sm font-medium">Бюджетоотримувач</label>
          <select id="input-budget-holder" class="w-full border rounded px-3 py-2 text-sm">
            <option value="">Оберіть бюджетоотримувача...</option>
            <option value="Відділ обліку персоналу">Відділ обліку персоналу</option>
            <option value="Департамент з експл. розподіл. мереж">Департамент з експл. розподіл. мереж</option>
            <option value="Відділ управління експлуатацією">Відділ управління експлуатацією</option>
          </select>
        </div>
        <!-- Прикріплення файлу -->
        <div class="flex items-center space-x-2 text-sm">
          <button id="attach-file-btn" class="flex items-center space-x-1 text-blue-500 hover:text-blue-600">
            <i data-lucide="paperclip"></i><span>Прикріпити файл</span>
          </button>
          <span id="attached-file-name" class="text-gray-600"></span>
          <input id="input-attachment" type="file" class="hidden" accept="image/*,application/pdf">
        </div>
      </div>
      <div class="flex justify-end space-x-3 p-6 border-t">
        <button id="modal-create-cancel" class="px-4 py-2 text-gray-600 hover:text-gray-800">Скасувати</button>
        <button id="modal-create-save" class="px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600">Зберегти</button>
      </div>
    </div>
  </div>

  <!-- Модальне вікно: Деталі витрати -->
  <div id="modal-expense-detail" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
    <div class="bg-white rounded-lg shadow-xl w-full max-w-xl mx-4">
      <div class="flex justify-between items-center p-6 border-b">
        <h2 class="text-lg font-semibold">Деталі витрати</h2>
        <button id="modal-detail-close" class="text-gray-400 hover:text-gray-600">
          <i data-lucide="x"></i>
        </button>
      </div>
      <div class="p-6 space-y-4">
        <div class="grid grid-cols-2 gap-4">
          <div class="space-y-2">
            <label class="block text-sm font-medium">Дирекція</label>
            <input id="detail-dept" type="text" class="w-full border rounded px-3 py-2 bg-gray-100" disabled>
          </div>
          <div class="space-y-2">
            <label class="block text-sm font-medium">Період</label>
            <input id="detail-period" type="text" class="w-full border rounded px-3 py-2 bg-gray-100" disabled>
          </div>
        </div>
        <div class="space-y-2">
          <label class="block text-sm font-medium">Сума</label>
          <input id="detail-amount" type="text" class="w-full border rounded px-3 py-2 bg-gray-100 text-blue-700 font-medium" disabled>
        </div>
        <div class="space-y-2">
          <label class="block text-sm font-medium">Опис</label>
          <textarea id="detail-description" class="w-full border rounded px-3 py-2 h-24 bg-gray-100" disabled></textarea>
        </div>
        <div class="space-y-2">
          <label class="block text-sm font-medium">Бюджетоотримувач</label>
          <input id="detail-budget-holder" type="text" class="w-full border rounded px-3 py-2 bg-gray-100" disabled>
        </div>
        <div id="detail-attachment-link" class="mt-2 text-blue-600 font-medium"></div>
        <div id="expert-file-section" class="mt-2 hidden">
          <div class="flex items-center space-x-2 text-sm">
            <button id="attach-file-detail-btn" class="flex items-center space-x-1 text-blue-500 hover:text-blue-600">
              <i data-lucide="paperclip"></i><span>Прикріпити файл</span>
            </button>
            <span id="attached-file-detail-name" class="text-gray-600"></span>
            <input id="input-attachment-detail" type="file" class="hidden" accept="image/*,application/pdf">
          </div>
        </div>
        <div id="expert-selection" class="mt-4 space-y-2" style="display: none;">
          <label class="block text-sm font-medium">Відповідальний експерт</label>
          <select id="detail-expert" class="w-full border rounded px-3 py-2 text-sm">
            <option value="">Оберіть експерта...</option>
            <option value="User1">User1</option>
            <option value="User2">User2</option>
            <option value="User3">User3</option>
          </select>
          <button id="assign-expert-btn" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">Призначити експерта</button>
        </div>
      </div>
      <div class="flex justify-end space-x-3 p-6 border-t">
        <button id="modal-detail-cancel" class="px-4 py-2 text-gray-600 hover:text-gray-800">Закрити</button>
        <button id="modal-detail-save" class="px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600 hidden">Зберегти зміни</button>
      </div>
    </div>
  </div>

  <!-- Модальне: Налаштування видимості -->
  <div id="modal-settings" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
    <div class="bg-white rounded-lg shadow-xl w-full max-w-4xl mx-4">
      <div class="flex justify-between items-center p-6 border-b">
        <h2 class="text-lg font-semibold">Налаштування видимості витрат</h2>
        <button id="modal-settings-close" class="text-gray-400 hover:text-gray-600">
          <i data-lucide="x"></i>
        </button>
      </div>
      <div class="p-6 space-y-4">
        <div class="flex items-center space-x-4 p-3 bg-blue-50 rounded">
          <span class="text-sm font-medium">Поточний відділ:</span>
          <select id="select-current-dept" class="border rounded px-3 py-2 text-sm">
            <option value="Відділ обліку персоналу">Відділ обліку персоналу</option>
            <option value="Відділ управління експлуатацією">Відділ управління експлуатацією</option>
            <option value="Департамент з експл. розподіл. мереж">Департамент з експл. розподіл. мереж</option>
          </select>
        </div>
        <p class="text-sm text-gray-600">Оберіть, які витрати будуть видимі для різних підрозділів</p>
        <div class="space-y-3 p-3 bg-gray-50 rounded">
          <h3 class="font-medium">Налаштування директорів</h3>
          <div class="grid grid-cols-2 gap-4">
            <div>
              <label class="block text-sm font-medium mb-1">Директор УА</label>
              <select id="select-director-ua" class="w-full border rounded px-3 py-2 text-sm">
                <option value="Іванов І.І.">Іванов І.І.</option>
                <option value="Петров П.П.">Петров П.П.</option>
                <option value="Сидоров С.С.">Сидоров С.С.</option>
              </select>
            </div>
            <div>
              <label class="block text-sm font-medium mb-1">Директор з напрямку</label>
              <select id="select-director-direction" class="w-full border rounded px-3 py-2 text-sm">
                <option value="Козлов К.К.">Козлов К.К.</option>
                <option value="Морозов М.М.">Морозов М.М.</option>
                <option value="Волков В.В.">Волков В.В.</option>
              </select>
            </div>
          </div>
        </div>
        <div id="settings-list" class="space-y-3"></div>
      </div>
      <div class="flex justify-end space-x-3 p-6 border-t">
        <button id="modal-settings-save" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">Зберегти</button>
      </div>
    </div>
  </div>

  <!-- Модальне: Відмова витрати -->
  <div id="modal-reject" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
    <div class="bg-white rounded-lg shadow-xl w-full max-w-lg mx-4">
      <div class="flex justify-between items-center p-6 border-b">
        <h2 class="text-lg font-semibold">Відмова витрати</h2>
        <button id="modal-reject-close" class="text-gray-400 hover:text-gray-600">
          <i data-lucide="x"></i>
        </button>
      </div>
      <div class="p-6">
        <div class="space-y-4">
          <label class="block text-sm font-medium">Коментар до відмови</label>
          <textarea id="input-reject-comment" class="w-full border rounded px-3 py-2 h-24" placeholder="Введіть причину..."></textarea>
        </div>
      </div>
      <div class="flex justify-end space-x-3 p-6 border-t">
        <button id="modal-reject-cancel" class="px-4 py-2 text-gray-600 hover:text-gray-800">Скасувати</button>
        <button id="modal-reject-save" class="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600">Підтвердити</button>
      </div>
    </div>
  </div>
