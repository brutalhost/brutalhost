При использовании IP адресов РФ не показывается гугл реклама, встроенный скрипт не находит ADS блок и насильно перезагружает страницу. Этот tampermonkey js убирает такое поведение.

```js
// ==UserScript==
// @name         Disable page reload at Perchance AI
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Disable page reload at Perchance AI
// @author       Andrey Orlov
// @match        https://perchance.org/*
// @run-at       document-start
// ==/UserScript==
 
// Отключение обновления страницы
(function () {
    window.addEventListener("beforeunload", function (event) {
        event.preventDefault();
        event.returnValue = false;
    });
 
    function removeElement(element) {
        element.parentNode.removeChild(element);
    }
 
    function checkAndRemoveElement(selector = ".ad-providers-ctn-el") {
        const childElement = document.querySelector(selector);
        if (childElement) {
            const parentElement = childElement.parentNode;
            removeElement(parentElement);
        }
    }
 
    let observer = new MutationObserver(function (mutations) {
        mutations.forEach(function (mutation) {
            mutation.addedNodes.forEach(function (node) {
                if (node.nodeType === Node.ELEMENT_NODE) {
                    checkAndRemoveElement();
                }
            });
        });
    });
 
    let config = {childList: true, subtree: true};
    observer.observe(document.body, config);
 
    window.addEventListener("load", function () {
        checkAndRemoveElement();
    });
})();
```