(function () {
  'use strict';

  function ownKeys(object, enumerableOnly) {
    var keys = Object.keys(object);

    if (Object.getOwnPropertySymbols) {
      var symbols = Object.getOwnPropertySymbols(object);
      enumerableOnly && (symbols = symbols.filter(function (sym) {
        return Object.getOwnPropertyDescriptor(object, sym).enumerable;
      })), keys.push.apply(keys, symbols);
    }

    return keys;
  }

  function _objectSpread2(target) {
    for (var i = 1; i < arguments.length; i++) {
      var source = null != arguments[i] ? arguments[i] : {};
      i % 2 ? ownKeys(Object(source), !0).forEach(function (key) {
        _defineProperty(target, key, source[key]);
      }) : Object.getOwnPropertyDescriptors ? Object.defineProperties(target, Object.getOwnPropertyDescriptors(source)) : ownKeys(Object(source)).forEach(function (key) {
        Object.defineProperty(target, key, Object.getOwnPropertyDescriptor(source, key));
      });
    }

    return target;
  }

  function _defineProperty(obj, key, value) {
    if (key in obj) {
      Object.defineProperty(obj, key, {
        value: value,
        enumerable: true,
        configurable: true,
        writable: true
      });
    } else {
      obj[key] = value;
    }

    return obj;
  }

  var _document = document,
      currentScript = _document.currentScript;
  var PUMBLE_DEFAULT_WIDGET_SRC = 'https://widget.pumble.com';
  var PUMBLE_DEFAULT_ZINDEX = 999995;
  var origin = currentScript.dataset.widgetSrc || PUMBLE_DEFAULT_WIDGET_SRC;
  var _theme = 'light';

  var _token;

  var _ifr;

  var iframeElementId = 'pumbleWidgetIFrame';

  function handlePostMessge(event) {
    var _event$data, _event$data2, _event$data3;

    if (event.origin !== origin || event.source !== _ifr.contentWindow) {
      return;
    }

    if (((_event$data = event.data) === null || _event$data === void 0 ? void 0 : _event$data.type) === 'RESTYLE' && (_event$data2 = event.data) !== null && _event$data2 !== void 0 && _event$data2.payload) {
      var payload = event.data.payload;
      Object.keys(payload).forEach(function (key) {
        _ifr.style[key] = payload[key];
      });
      return;
    }

    if (((_event$data3 = event.data) === null || _event$data3 === void 0 ? void 0 : _event$data3.type) === 'UNMOUNT') {
      window.$Pmbl.unmount();
    }
  }

  function sendPostMessage(data) {
    if (!_ifr) {
      return;
    }

    _ifr.contentWindow.postMessage(_objectSpread2({}, data), origin);
  }

  window.$Pmbl = function () {
    var insertStyleElement = function insertStyleElement() {
      var styleElement = document.createElement('style');
      styleElement.innerHTML = "\n\t\t\t#".concat(iframeElementId, " {\n\t\t\t\tmargin: 0;\n\t\t\t\tborder: none;\n\t\t\t\toutline: none;\n\t\t\t\tpadding: 0;\n\t\t\t\tdisplay: none;\n\t\t\t\ttransition: box-shadow 0.12s ease;\n\t\t\t}\n\t\t").replace(/\n/g, '').replace(/\s+/g, ' ');
      document.head.appendChild(styleElement);
    };

    return {
      mount: function mount() {
        var _ref = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : {},
            theme = _ref.theme,
            token = _ref.token,
            _ref$zindex = _ref.zindex,
            zindex = _ref$zindex === void 0 ? PUMBLE_DEFAULT_ZINDEX : _ref$zindex;

        if (_ifr) return;
        insertStyleElement();
        _ifr = document.createElement('iframe');
        var ifrSrc = new URL(origin);
        var searchParams = new URLSearchParams();
        searchParams.append('token', token || _token);
        searchParams.append('theme', theme || _theme);
        searchParams.append('zindex', zindex);
        searchParams.append('origin', window.location.origin);
        ifrSrc.search = searchParams.toString();

        _ifr.setAttribute('src', ifrSrc);

        _ifr.setAttribute('id', iframeElementId);

        _ifr.setAttribute('allow', 'clipboard-write');

        document.body.appendChild(_ifr);
        window.addEventListener('message', handlePostMessge);
      },
      unmount: function unmount() {
        if (!_ifr) return;
        window.removeEventListener('message', handlePostMessge);

        _ifr.remove();

        _ifr = null;
      },
      setTheme: function setTheme() {
        var theme = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : '';
        if (!_ifr) return;
        sendPostMessage({
          type: 'SET_THEME',
          theme: theme
        });
      },
      setOpen: function setOpen(isOpen) {
        if (!_ifr || typeof isOpen === 'undefined') {
          return;
        }

        sendPostMessage({
          type: 'SET_OPEN',
          isOpen: Boolean(isOpen)
        });
      }
    };
  }();

  if (currentScript.dataset.token) {
    var _currentScript$datase = currentScript.dataset,
        theme = _currentScript$datase.theme,
        token = _currentScript$datase.token,
        zindex = _currentScript$datase.zindex;
    _token = token;
    window.$Pmbl.mount({
      theme: theme,
      token: token,
      zindex: zindex
    });
  }

}());
