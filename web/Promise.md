Promise

[Promise A+](https://promisesaplus.com)

[Promise A+中文](https://www.ituring.com.cn/article/66566)

``` javascript
class Promise {
    constructor(handle) {
        this.stateMap = {
            PENDING: "PENDING",
            FULFILLED: "FULFILLED",
            REJECTED: "REJECTED",
        };
        this.state = this.stateMap.PENDING;

        this._value = null;
        this.callbacks = [];

        try {
            handle(this._resolved.bind(this), this._rejected.bind(this));
        } catch (error) {
            this._rejected(error);
        }
    }

    _resolved(v) {
        // 返回值是一个新的promise
        if (v && v instanceof Promise) {
            let then = v.then;
            then.call(v, this._resolved.bind(this), this._rejected.bind(this));
            return;
        }

        this.state = this.stateMap.FULFILLED;
        this._value = v;
        this.callbacks.forEach((callback) => this._handle(callback));
    }

    _rejected(v) {
        this.state = this.stateMap.REJECTED;
        this._value = v;
        this.callbacks.forEach((callback) => this._handle(callback));
    }

    _handle(callback) {
        if (this.state === this.stateMap.PENDING) {
            this.callbacks.push(callback);
            return;
        }

        if (this.state === this.stateMap.FULFILLED) {
            if (!callback.onFulfilled) {
                callback.resolve(this._value);
                return;
            }
            try {
                let result = callback.onFulfilled(this._value);
                callback.resolve(result);
            } catch (error) {
                callback.reject(error);
            }
        } else {
            if (!callback.onRejected) {
                callback.reject(this._value);
                return;
            }
            try {
                let result = callback.onRejected(this._value);
                callback.resolve(result);
            } catch (error) {
                callback.reject(error);
            }
        }
    }

    then(onFulfilled, onRejected) {
        return new Promise((resolve, reject) => {
            this._handle({
                onFulfilled,
                onRejected,
                resolve,
                reject,
            });
        });
    }

    catch(onRejected) {
        return this.then(null, onRejected);
    }

    finally(onFinal) {
        return this.then(
            (value) => Promise.resolve(onFinal()).then(() => value),
            (reason) =>
                Promise.resolve(onFinal()).then(() => {
                    throw reason;
                })
        );
    }

    static resolve(v) {
        // promise A+ 规范：
        // resolve可以传入promise， thenable等
        if (v && v instanceof Promise) {
            return v;
        } else if (v && typeof v === "object" && typeof v.then === "function") {
            let then = v.then;
            return new Promise((resolve) => {
                then(resolve);
            });
        } else {
            return new Promise((resolve) => {
                resolve(v);
            });
        }
    }

    static reject(v) {
        if (v && typeof v === "object" && typeof v.then === "function") {
            let then = v.then;
            return new Promise((resolve, reject) => {
                then(reject);
            });
        } else {
            return new Promise((resolve, reject) => reject(value));
        }
    }

    static all(promises) {
        return new Promise((resolve, reject) => {
            let leng = promises.length,
                resArr = Array.from({ length: leng }),
                count = 0;
            promises.forEach((promise, i) => {
                promise.then(
                    (res) => {
                        count++;
                        resArr[i] = res;
                        if (count === leng) {
                            resolve(resArr);
                        }
                    },
                    (err) => {
                        reject(err);
                    }
                );
            });
        });
    }

    static race(promises) {
        return new Promise((resolve, reject) => {
            for (let i = 0; i < promises.length; i++) {
                Promise.resolve(promises[i]).then(
                    (value) => {
                        return resolve(value);
                    },
                    (reason) => {
                        return reject(reason);
                    }
                );
            }
        });
    }
}

// let s = Promise.resolve('1');
// s.then(r => {
//     return s
// })

// Promise.race([Promise.resolve("32"), Promise.resolve("45")])
//     .then((res) => {
//         console.log("----->", res);
//     })
//     .catch((err) => {
//         console.log("----->e", err);
//     });

// HPromise.resolve('s').then(res => {
//     // throw('1')
//     console.log('----->0', res);
// }).then(res => {
//     console.log('----->1', res);
// }).catch(err => {
//     console.log('----->r1', err);
// }).then(res => {
//     console.log('----->2', 'ss');
//     throw new Error('ff')
// }, err => {
//     console.log('----->', 'dd');
// }).catch(err=> {
//     console.log('----->5', err.message);
// })

// new Promise((resolve, reject) => {
//     setTimeout(() => {
//         reject("fds");
//     }, 1000);
// })
//     .finally((r) => {
//         console.log("----->fi", r);
//     })
//     .then((r) => {
//         console.log("----->", r);
//     });

```

