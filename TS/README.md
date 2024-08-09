# TYPESCRIPT

## The compiler
Compiler detects errors and convert ts to js : called tsc

## Compiler configuration
`tsc --init` creates a tsconfig.json
Enable no unused parameters, ...

## How to debug ?
Enable sourcemap in tsconfig.json
In debug in VSCode, create launch.json
Add preLaunchTask (see file)

## Random examples
[Link](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

Type inference exists `let nb = 5` is correct

```
let nb: number = 123_456_789;
let msg: string = 'Hello';
let is_ok: boolean = true;
let arr: number[] = [1, 2, 3];
let bad; -> any type
let tuple: [number, string] = [1, 'GP'];

enum Size {
    Small, Medium, Large
}; -> by default, assign 0 to the first value and so on
let mySize: Size = Size.Large;

type Person = {
    readonly id: number,
    name: string
};
let me: Person = {
    id: 1,
    name: 'Lucas'
};

let bar: number | string; -> union type, OR
let bar: Draggable & Resizable; -> intersection type, AND

type literal = 50 | 100;

number | null; for nullable

me?.birthday -> optional property access operator, if property doesn't exists, return undefined

list?.[0] -> optional element access operator, check not empty or undefined

let log: any = null;
log?.('message'); -> optional call operator

const bar: string = null ?? 'default string'; -> nullish coalescing operator

function foo(nb: number): string {
    return 'Good job';
}

```