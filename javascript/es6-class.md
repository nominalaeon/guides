<img src="../_images/javascript-badge.png" alt="JavaScript badge" title="JavaScript badge" style="max-height: 260px;"/>

# ECMAScript 6 Class

* * *

- [Organizing](#organizing)

* * *

<a id="markdown-organizing" name="organizing"></a>
## Organizing

    (() => {

        'use strict'; /** because you're not a monster. */

        class ExampleClass {

            constructor(example) {
                this.init.apply(this, arguments);
            }

            get firstName() {
                return this._vm.firstName || '';
            }
            set firstName(firstName) {
                this._vm.firstName = firstName;
            }

            get fullName() {
                return this._vm.fullName || `${this.firstName} ${this.lastName}`;
            }
            set fullName(fullName) {
                this._vm.fullName = fullName;
            }

            get id() {
                return this._vm.id || null;
            }
            set id(id) {
                this._vm.id = id;
            }

            get lastName() {
                return this._vm.lastName || '';
            }
            set lastName(lastName) {
                this._vm.lastName = lastName;
            }

            get luckyNumber() {
                return this._vm.luckyNumber || _.random(1, 1000);
            }
            set luckyNumber(luckyNumber) {
                this._vm.luckyNumber = luckyNumber;
            }

            addExample() { ... }

            assignExamples() { ... }

            collectExamples() { ... }

            deleteExample() { ... }

            init(example) {
                this._vm = {};

                for (var prop in example) {
                    if (!example.hasOwnProperty(prop)) {
                        continue;
                    }
                    this[prop] = example[prop];
                }
            }

        }

    })();
