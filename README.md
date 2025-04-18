# Chrome拡張機能で新規タブページを置き換える詳細ガイド

## 新規タブページを置き換える完全ガイド

Chrome拡張機能で新規タブページ（chrome://newtab）を完全に独自のものに置き換えることができます。以下に詳細な手順を説明します。

### 1. manifest.json の設定

```json
{
  "name": "Custom New Tab with Theme",
  "version": "1.0",
  "description": "カスタム新規タブページとテーマを組み合わせた拡張機能",
  "manifest_version": 3,
  "chrome_url_overrides": {
    "newtab": "newtab.html"
  },
  "permissions": [],
  "icons": {
    "16": "icons/icon16.png",
    "48": "icons/icon48.png",
    "128": "icons/icon128.png"
  },
  "theme": {
    // テーマの設定（既存のテーマ設定をここに記述）
  }
}
```

`chrome_url_overrides` の `newtab` キーに HTML ファイルを指定することで、新規タブを開いたときに表示されるページを完全に置き換えられます。

### 2. 基本的な newtab.html の作成

```html
<!DOCTYPE html>
<html>
<head>
  <title>My New Tab</title>
  <link rel="stylesheet" href="newtab.css">
</head>
<body>
  <div class="container">
    <div class="search-container">
      <form action="https://www.google.com/search" method="get">
        <!-- Googleロゴなしの検索ボックス -->
        <input type="text" name="q" placeholder="Google検索" autofocus>
        <button type="submit">検索</button>
      </form>
    </div>
    
    <div class="bookmarks">
      <!-- よく使うサイトへのリンク -->
      <div class="bookmark-item">
        <a href="https://www.example.com">
          <div class="bookmark-icon">E</div>
          <div class="bookmark-title">Example</div>
        </a>
      </div>
      <!-- 他のブックマークアイテム -->
    </div>
    
    <div class="custom-widgets">
      <!-- 時計ウィジェット -->
      <div class="clock-widget">
        <div id="time">00:00:00</div>
        <div id="date">2023年1月1日</div>
      </div>
      
      <!-- 天気ウィジェットなど -->
    </div>
  </div>
  
  <script src="newtab.js"></script>
</body>
</html>
```

### 3. スタイル設定 (newtab.css)

```css
body {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
  background-color: #f5f5f5;
  /* または背景画像を使用 */
  background-image: url('images/background.jpg');
  background-size: cover;
  background-position: center;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container {
  width: 80%;
  max-width: 800px;
}

.search-container {
  margin-bottom: 30px;
  text-align: center;
}

.search-container input {
  width: 70%;
  padding: 12px 15px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 24px;
  outline: none;
}

.search-container button {
  padding: 12px 20px;
  background-color: #4285f4;
  color: white;
  border: none;
  border-radius: 24px;
  margin-left: 10px;
  cursor: pointer;
}

.bookmarks {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  margin-bottom: 40px;
}

.bookmark-item {
  width: 100px;
  text-align: center;
}

.bookmark-icon {
  width: 48px;
  height: 48px;
  background-color: #e0e0e0;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 8px;
  font-size: 20px;
  color: #333;
}

.bookmark-title {
  font-size: 14px;
  color: #333;
}

.clock-widget {
  text-align: center;
  margin-top: 40px;
}

#time {
  font-size: 48px;
  font-weight: bold;
  color: #333;
}

#date {
  font-size: 18px;
  color: #666;
}
```

### 4. JavaScript機能 (newtab.js)

```javascript
// 時計機能
function updateClock() {
  const now = new Date();
  
  // 時間表示
  const hours = String(now.getHours()).padStart(2, '0');
  const minutes = String(now.getMinutes()).padStart(2, '0');
  const seconds = String(now.getSeconds()).padStart(2, '0');
  document.getElementById('time').textContent = `${hours}:${minutes}:${seconds}`;
  
  // 日付表示
  const year = now.getFullYear();
  const month = now.getMonth() + 1;
  const day = now.getDate();
  document.getElementById('date').textContent = `${year}年${month}月${day}日`;
}

// 1秒ごとに時計を更新
setInterval(updateClock, 1000);
updateClock(); // 初期表示

// ブックマークを動的に読み込む（オプション）
function loadBookmarks() {
  // 例: ローカルストレージからブックマークを読み込む
  const bookmarks = [
    { title: 'Google', url: 'https://www.google.com', icon: 'G' },
    { title: 'YouTube', url: 'https://www.youtube.com', icon: 'Y' },
    { title: 'Gmail', url: 'https://mail.google.com', icon: 'M' },
    // 他のブックマーク
  ];
  
  const bookmarksContainer = document.querySelector('.bookmarks');
  
  bookmarks.forEach(bookmark => {
    const bookmarkItem = document.createElement('div');
    bookmarkItem.className = 'bookmark-item';
    
    bookmarkItem.innerHTML = `
      <a href="${bookmark.url}">
        <div class="bookmark-icon">${bookmark.icon}</div>
        <div class="bookmark-title">${bookmark.title}</div>
      </a>
    `;
    
    bookmarksContainer.appendChild(bookmarkItem);
  });
}

// ページ読み込み時にブックマークを表示
document.addEventListener('DOMContentLoaded', loadBookmarks);

// 背景画像をランダムに変更する（オプション）
function setRandomBackground() {
  const backgrounds = [
    'background1.jpg',
    'background2.jpg',
    'background3.jpg'
  ];
  
  const randomBg = backgrounds[Math.floor(Math.random() * backgrounds.length)];
  document.body.style.backgroundImage = `url('images/${randomBg}')`;
}

// ランダム背景を適用（オプション）
// setRandomBackground();
```

### 5. 高度なカスタマイズオプション

#### 5.1 Chrome APIの利用

より高度な機能を実装するには、追加の権限が必要です：

```json
// manifest.jsonに追加
"permissions": [
  "bookmarks",
  "topSites",
  "storage"
],
```

#### 5.2 実際のブックマークを表示

```javascript
// newtab.jsに追加
function loadChromeBookmarks() {
  chrome.bookmarks.getRecent(10, function(bookmarks) {
    const bookmarksContainer = document.querySelector('.bookmarks');
    bookmarksContainer.innerHTML = '';
    
    bookmarks.forEach(bookmark => {
      const bookmarkItem = document.createElement('div');
      bookmarkItem.className = 'bookmark-item';
      
      const firstLetter = bookmark.title.charAt(0).toUpperCase();
      
      bookmarkItem.innerHTML = `
        <a href="${bookmark.url}">
          <div class="bookmark-icon">${firstLetter}</div>
          <div class="bookmark-title">${bookmark.title}</div>
        </a>
      `;
      
      bookmarksContainer.appendChild(bookmarkItem);
    });
  });
}

// ページ読み込み時に実行
document.addEventListener('DOMContentLoaded', loadChromeBookmarks);
```

#### 5.3 よく訪問するサイトを表示

```javascript
// newtab.jsに追加
function loadTopSites() {
  chrome.topSites.get(function(sites) {
    const topSitesContainer = document.createElement('div');
    topSitesContainer.className = 'top-sites';
    document.querySelector('.container').appendChild(topSitesContainer);
    
    sites.forEach(site => {
      // サイトアイコンを取得するためのURLを作成
      const faviconUrl = `https://www.google.com/s2/favicons?domain=${new URL(site.url).hostname}`;
      
      const siteItem = document.createElement('div');
      siteItem.className = 'site-item';
      
      siteItem.innerHTML = `
        <a href="${site.url}">
          <div class="site-icon">
            <img src="${faviconUrl}" alt="${site.title}">
          </div>
          <div class="site-title">${site.title}</div>
        </a>
      `;
      
      topSitesContainer.appendChild(siteItem);
    });
  });
}
```

### 6. ユーザー設定を保存する

```javascript
// newtab.jsに追加
// 設定を保存
function saveSettings(settings) {
  chrome.storage.sync.set({ 'newTabSettings': settings }, function() {
    console.log('設定が保存されました');
  });
}

// 設定を読み込む
function loadSettings(callback) {
  chrome.storage.sync.get('newTabSettings', function(data) {
    if (data.newTabSettings) {
      callback(data.newTabSettings);
    } else {
      // デフォルト設定
      callback({
        backgroundColor: '#f5f5f5',
        useBackgroundImage: true,
        backgroundImage: 'background1.jpg',
        showClock: true,
        showBookmarks: true
      });
    }
  });
}

// 設定を適用
function applySettings(settings) {
  if (settings.useBackgroundImage) {
    document.body.style.backgroundImage = `url('images/${settings.backgroundImage}')`;
  } else {
    document.body.style.backgroundColor = settings.backgroundColor;
    document.body.style.backgroundImage = 'none';
  }
  
  document.querySelector('.clock-widget').style.display = 
    settings.showClock ? 'block' : 'none';
  
  document.querySelector('.bookmarks').style.display = 
    settings.showBookmarks ? 'flex' : 'none';
}

// 設定を読み込んで適用
document.addEventListener('DOMContentLoaded', function() {
  loadSettings(applySettings);
});
```

### 7. 設定ページの追加（オプション）

`options.html`と`options.js`を作成し、manifest.jsonに以下を追加:

```json
"options_ui": {
  "page": "options.html",
  "open_in_tab": false
}
```

これにより、ユーザーが新規タブページの見た目や機能をカスタマイズできるようになります。

### 8. 注意点

- 新規タブページを置き換えると、元のChromeの新規タブページの機能（Google検索、おすすめサイト等）は使えなくなります
- 必要な機能は自分で実装する必要があります
- 拡張機能のサイズが大きくなりすぎないよう注意（画像は最適化する）
- テーマ設定と新規タブページの置き換えは併用可能です

この方法で、Googleロゴのない完全にカスタマイズされた新規タブページを作成できます。
