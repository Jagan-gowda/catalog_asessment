
function decode(value, base) {
    let decodedValue = 0;
    let power = 0;

    while (value > 0) {
        const digit = value % 10;
        decodedValue += digit * Math.pow(base, power);
        value = Math.floor(value / 10);
        power++;
    }

    return decodedValue;
}


function lagrangeInterpolation(x, y, x0) {
    let result = 0.0;
    const n = x.length;

    for (let i = 0; i < n; i++) {
        let term = y[i];
        for (let j = 0; j < n; j++) {
            if (i !== j) {
                term *= (x0 - x[j]) / (x[i] - x[j]);
            }
        }
        result += term;
    }

    return result;
}


function main() {
    const jsonInput = `{
        "keys": {
            "n": 4,
            "k": 3
        },
        "1": {
            "base": "10",
            "value": "4"
        },
        "2": {
            "base": "2",
            "value": "111"
        },
        "3": {
            "base": "10",
            "value": "12"
        },
        "6": {
            "base": "4",
            "value": "213"
        }
    }`;

    const json = JSON.parse(jsonInput);
    const points = {};

  
    for (const key in json) {
        if (key === "keys") continue;
        const point = json[key];
        const base = parseInt(point.base);
        const value = parseInt(point.value);
        const decodedValue = decode(value, base);
        points[parseInt(key)] = decodedValue;
    }

    
    const x = Object.keys(points).map(Number);
    const y = Object.values(points);

    // Calculate the constant term c (f(0))
    const constantTerm = lagrangeInterpolation(x, y, 0);
    console.log("Constant term (c):", constantTerm);
}

A
main();
