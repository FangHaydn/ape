# Vue.nextTickets实现原理

Vue.nextTickets方法可以实现在下次DOM更新后调用callback，实现更新后的DOM操作。

主要实现方式是eventloop中的微任务（microtask queue），默认使用Promise中的then方法，降级处理使用MutationObserver实现，再降级使用宏任务实现（macrotask queue）setImmediate和setTimeout

```javascript
export function nextTick(cb?: Function, ctx?: Object) {
    let _resolve;
    callbacks.push(() => {
        if (cb) {
            try {
                cb.call(ctx);
            } catch (e) {
                handleError(e, ctx, "nextTick");
            }
        } else if (_resolve) {
            _resolve(ctx);
        }
    });
    if (!pending) {
        pending = true;
        timerFunc();
    }
    // $flow-disable-line
    if (!cb && typeof Promise !== "undefined") {
        return new Promise((resolve) => {
            _resolve = resolve;
        });
    }
}

function timerFunc() {
    if (typeof Promise !== "undefined" && isNative(Promise)) {
        const p = Promise.resolve();
        timerFunc = () => {
            p.then(flushCallbacks);
            // In problematic UIWebViews, Promise.then doesn't completely break, but
            // it can get stuck in a weird state where callbacks are pushed into the
            // microtask queue but the queue isn't being flushed, until the browser
            // needs to do some other work, e.g. handle a timer. Therefore we can
            // "force" the microtask queue to be flushed by adding an empty timer.
            if (isIOS) setTimeout(noop);
        };
        isUsingMicroTask = true;
    } else if (
        !isIE &&
        typeof MutationObserver !== "undefined" &&
        (isNative(MutationObserver) ||
            // PhantomJS and iOS 7.x
            MutationObserver.toString() ===
                "[object MutationObserverConstructor]")
    ) {
        // Use MutationObserver where native Promise is not available,
        // e.g. PhantomJS, iOS7, Android 4.4
        // (#6466 MutationObserver is unreliable in IE11)
        let counter = 1;
        const observer = new MutationObserver(flushCallbacks);
        const textNode = document.createTextNode(String(counter));
        observer.observe(textNode, {
            characterData: true,
        });
        timerFunc = () => {
            counter = (counter + 1) % 2;
            textNode.data = String(counter);
        };
        isUsingMicroTask = true;
    } else if (typeof setImmediate !== "undefined" && isNative(setImmediate)) {
        // Fallback to setImmediate.
        // Techinically it leverages the (macro) task queue,
        // but it is still a better choice than setTimeout.
        timerFunc = () => {
            setImmediate(flushCallbacks);
        };
    } else {
        // Fallback to setTimeout.
        timerFunc = () => {
            setTimeout(flushCallbacks, 0);
        };
    }
}

function flushCallbacks() {
    pending = false;
    const copies = callbacks.slice(0);
    callbacks.length = 0;
    for (let i = 0; i < copies.length; i++) {
        copies[i]();
    }
}
```
