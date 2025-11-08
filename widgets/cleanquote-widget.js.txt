// cleanquote-widget.js
(function(){
  var script = document.currentScript || (function(){
    var s = document.getElementsByTagName('script');
    return s[s.length - 1];
  })();

  var proName = script.getAttribute('data-pro-name') || 'Your Pressure Washing Co.';
  var primary = script.getAttribute('data-primary-color') || '#21c78b';
  var endpoint = script.getAttribute('data-contact-endpoint') || '';
  var mountId = script.getAttribute('data-target-id') || 'cardinal-cleanquote-widget';

  var mount = document.getElementById(mountId);
  if (!mount) {
    mount = document.createElement('div');
    mount.id = mountId;
    script.parentNode.insertBefore(mount, script);
  }

  var css = `
  .ccq-card{
    font-family:system-ui,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;
    background:#050809;
    color:#f4f7f5;
    border-radius:18px;
    padding:12px 12px 10px;
    border:1px solid rgba(255,255,255,.14);
    box-shadow:0 12px 30px rgba(0,0,0,.72);
    max-width:320px;
  }
  .ccq-header{
    display:flex;
    justify-content:space-between;
    align-items:flex-start;
    gap:8px;
    margin-bottom:4px;
  }
  .ccq-title{
    font-size:12px;
    font-weight:600;
  }
  .ccq-sub{
    font-size:9px;
    color:rgba(244,247,245,.7);
  }
  .ccq-tag{
    font-size:8px;
    padding:3px 7px;
    border-radius:999px;
    border:1px solid rgba(255,255,255,.24);
    color:rgba(244,247,245,.8);
  }
  .ccq-label{
    display:block;
    font-size:8px;
    margin:3px 0 1px;
    color:rgba(244,247,245,.72);
  }
  .ccq-select,
  .ccq-input,
  .ccq-file{
    width:100%;
    padding:5px 7px;
    border-radius:8px;
    border:1px solid rgba(255,255,255,.18);
    background:#020404;
    color:#f4f7f5;
    font-size:9px;
  }
  .ccq-row{
    display:flex;
    gap:4px;
  }
  .ccq-grime-row{
    display:flex;
    gap:4px;
    margin-top:2px;
  }
  .ccq-pill{
    flex:1;
    text-align:center;
    padding:3px 4px;
    font-size:7px;
    border-radius:999px;
    border:1px solid rgba(255,255,255,.18);
    cursor:pointer;
    opacity:.7;
  }
  .ccq-pill.ccq-active{
    border-color:`+primary+`;
    opacity:1;
  }
  .ccq-btn{
    width:100%;
    margin-top:5px;
    padding:6px;
    border-radius:9px;
    border:none;
    background:`+primary+`;
    color:#020404;
    font-size:9px;
    font-weight:600;
    cursor:pointer;
  }
  .ccq-output{
    margin-top:5px;
    padding:5px;
    border-radius:9px;
    border:1px solid rgba(255,255,255,.14);
    background:#020404;
    font-size:8px;
  }
  .ccq-package{
    display:flex;
    justify-content:space-between;
    font-size:8px;
    margin:1px 0;
  }
  .ccq-price{
    color:`+primary+`;
    font-weight:600;
  }
  .ccq-foot{
    margin-top:3px;
    font-size:7px;
    color:rgba(244,247,245,.55);
  }`;

  var style = document.createElement('style');
  style.type = 'text/css';
  style.appendChild(document.createTextNode(css));
  document.head.appendChild(style);

  mount.innerHTML = ''
    + '<div class="ccq-card">'
    + '  <div class="ccq-header">'
    + '    <div>'
    + '      <div class="ccq-title">Instant Pressure Wash Quote</div>'
    + '      <div class="ccq-sub">Powered by '+ proName +'</div>'
    + '    </div>'
    + '    <div class="ccq-tag">30 sec • No signup</div>'
    + '  </div>'
    + '  <label class="ccq-label">Surface</label>'
    + '  <select class="ccq-select" id="ccq-surface">'
    + '    <option value="driveway">Driveway</option>'
    + '    <option value="patio">Patio / Walk</option>'
    + '    <option value="house">House Siding</option>'
    + '    <option value="fence">Fence</option>'
    + '  </select>'
    + '  <div class="ccq-row">'
    + '    <div style="flex:1;">'
    + '      <label class="ccq-label">Length (ft)</label>'
    + '      <input type="number" id="ccq-length" class="ccq-input" value="40" min="5" />'
    + '    </div>'
    + '    <div style="flex:1;">'
    + '      <label class="ccq-label">Width (ft)</label>'
    + '      <input type="number" id="ccq-width" class="ccq-input" value="10" min="3" />'
    + '    </div>'
    + '  </div>'
    + '  <label class="ccq-label">Grime level</label>'
    + '  <div class="ccq-grime-row" id="ccq-grime">'
    + '    <div class="ccq-pill ccq-active" data-mult="0.9">Light</div>'
    + '    <div class="ccq-pill" data-mult="1.0">Average</div>'
    + '    <div class="ccq-pill" data-mult="1.2">Heavy</div>'
    + '  </div>'
    + '  <label class="ccq-label">Optional photo</label>'
    + '  <input type="file" id="ccq-photo" class="ccq-file" accept="image/*" />'
    + '  <label class="ccq-label">Name &amp; phone</label>'
    + '  <input type="text" id="ccq-name" class="ccq-input" placeholder="So we can text your quote" />'
    + '  <button class="ccq-btn" id="ccq-btn">Get instant quote</button>'
    + '  <div class="ccq-output" id="ccq-out" style="display:none;">'
    + '    <div class="ccq-package"><div>Good • Basic Wash</div><div class="ccq-price" id="ccq-good"></div></div>'
    + '    <div class="ccq-package"><div>Better • Deep Clean</div><div class="ccq-price" id="ccq-better"></div></div>'
    + '    <div class="ccq-package"><div>Best • Premium + Seal</div><div class="ccq-price" id="ccq-best"></div></div>'
    + '    <div class="ccq-foot">You\'ll get a confirmation from '+ proName +'. Prices are ballpark until on-site.</div>'
    + '  </div>'
    + '</div>';

  var surfaceEl = mount.querySelector('#ccq-surface');
  var lengthEl = mount.querySelector('#ccq-length');
  var widthEl = mount.querySelector('#ccq-width');
  var grimeRow = mount.querySelector('#ccq-grime');
  var btn = mount.querySelector('#ccq-btn');
  var out = mount.querySelector('#ccq-out');
  var goodEl = mount.querySelector('#ccq-good');
  var betterEl = mount.querySelector('#ccq-better');
  var bestEl = mount.querySelector('#ccq-best');
  var nameEl = mount.querySelector('#ccq-name');

  var grimeMult = 0.9;

  grimeRow.addEventListener('click', function(e){
    var pill = e.target.closest('.ccq-pill');
    if(!pill) return;
    grimeMult = parseFloat(pill.getAttribute('data-mult') || '1');
    Array.prototype.forEach.call(grimeRow.children, function(p){ p.classList.remove('ccq-active'); });
    pill.classList.add('ccq-active');
  });

  function clamp(n, min, max){
    n = Number(n);
    if(!isFinite(n)) n = 0;
    if(n < min) n = min;
    if(n > max) n = max;
    return n;
  }

  function calcBase(surface){
    if(surface === 'driveway' || surface === 'patio') return {low:0.10, high:0.18};
    if(surface === 'house') return {low:0.15, high:0.25};
    if(surface === 'fence') return {low:0.80, high:1.50}; // per linear foot
    return {low:0.12, high:0.20};
  }

  function usd(v){
    return '$' + v.toFixed(0);
  }

  btn.addEventListener('click', function(){
    var surface = surfaceEl.value;
    var L = clamp(lengthEl.value || 0, 5, 500);
    var W = clamp(widthEl.value || 0, 3, 200);

    var area = L * W;
    if(surface === 'fence') area = L;

    var base = calcBase(surface);
    var low = area * base.low * grimeMult;
    var high = area * base.high * grimeMult;

    if(low < 40) low = 40;
    if(high < low + 20) high = low + 20;

    var goodLow = low * 0.9;
    var goodHigh = low;
    var betterLow = low;
    var betterHigh = (low + high) / 2;
    var bestLow = betterHigh;
    var bestHigh = high * 1.15;

    goodEl.textContent = usd(goodLow) + ' – ' + usd(goodHigh);
    betterEl.textContent = usd(betterLow) + ' – ' + usd(betterHigh);
    bestEl.textContent = usd(bestLow) + ' – ' + usd(bestHigh);

    out.style.display = 'block';

    if(endpoint){
      var payload = {
        proName: proName,
        surface: surface,
        length: L,
        width: W,
        grimeMultiplier: grimeMult,
        quote: {
          good: {low: goodLow, high: goodHigh},
          better: {low: betterLow, high: betterHigh},
          best: {low: bestLow, high: bestHigh}
        },
        customerName: nameEl.value || '',
        ts: new Date().toISOString()
      };

      try{
        fetch(endpoint, {
          method:'POST',
          headers:{'Content-Type':'application/json'},
          body: JSON.stringify(payload)
        }).catch(function(){});
      }catch(e){}
    }
  });
})();
