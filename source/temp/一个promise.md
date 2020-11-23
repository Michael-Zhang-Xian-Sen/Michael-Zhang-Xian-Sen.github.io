```js
class MyPromise {
    static PENDING = 'pending'
    static RESOLVED = 'resolved'
    static REJECTED = 'rejected'

    constructor(fn) {
        typeof fn === 'function' ? fn(this.resolve.bind(this), this.reject.bind(this)) : null;
        this.state = MyPromise.PENDING;
        this.value = null;
        this.newPromise = null;
        this.resolvedCallback = null;
        this.rejectedCallback = null;
    }

    then(onFulfilled) {
        this.resolvedCallback = onFulfilled;
        this.newPromise = new MyPromise();
        return this.newPromise;
    }

    catch(onRejected) {
        this.rejectedCallback = onRejected;
    }

    resolve(value) {
        if (this.state !== MyPromise.PENDING) return false;
        this.value = value;
        this.state = MyPromise.RESOLVED;
        if (this.newPromise) this.newPromise.resolve(this.resolvedCallback(value));
    }

    reject(err) {
        if (this.state !== MyPromise.PENDING) return false;
        this.value = err;
        this.state = MyPromise.REJECTED;
        this.rejectedCallbacks(err);
    }
}
const promise = new MyPromise((resolve, reject) => {
    setTimeout(resolve, 1000, 'hello');
});

promise.then(res => {
    console.log(res);
    res += ' world';
    return res;
}).then(res => {
    console.log(res);
    res += '!';
    return res;
}).then(res => {
    console.log(res);
}).catch(e => {
    console.error(e);
});
```