<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
        <style>
            .show {
                display: flex;
                justify-content: space-around;
            }
        </style>
    </head>
    <body>
        <select id="dheeraj" onchange="chg()"></select>
        <div id="displayData"></div>
        <script>
            // let Newdata;
            // let arr;
            display = (arr) => {
                arr.sort((a, b) => {
                    return b.Value - a.Value;
                });
                let card = document.getElementById("displayData");
                card.innerHTML = "";
                for (let i = 0; i < 10; i++) {
                    let c = document.createElement("div");
                    c.setAttribute("class", "show");

                    let h2 = document.createElement("h2");
                    h2.textContent = arr[i]["Country Name"];
                    let p = document.createElement("p");
                    p.textContent = `Year : ${arr[i].Year}`;
                    let h3 = document.createElement("h3");
                    h3.textContent = `Population : ${arr[i].Value}`;
                    c.append(h2, p, h3);
                    card.append(c);
                }
            };
            getValue = (data) => {
                let val = document.getElementById("dheeraj").value;
                let start = 1960;
                let id = setInterval(() => {
                    if (start == val) {
                        clearInterval(id);
                    }
                    let arr = data.filter((ele) => {
                        return start == ele.Year;
                    });
                    display(arr);
                    start++;
                    val + 1;
                }, 300);
            };

            chg = () => {
                fetch(
                    "https://codejudge-question-artifacts.s3.ap-south-1.amazonaws.com/poplution-countries-yearwise.json"
                )
                    .then((res) => res.json())
                    .then((res) => {
                        getValue(res);
                    });
            };

            showOption = (data) => {
                data.sort((a, b) => a.Year - b.Year);
                let select = document.getElementById("dheeraj");
                let pre;
                for (let i = 0; i < data.length; i++) {
                    // Newdata.push(data[i]);

                    ele = data[i].Year;
                    if (ele === pre) {
                        continue;
                    }
                    let opt = document.createElement("option");
                    opt.value = ele;
                    opt.textContent = ele;
                    select.append(opt);
                    pre = ele;
                }
            };
            getData = async () => {
                let res = await fetch(
                    "https://codejudge-question-artifacts.s3.ap-south-1.amazonaws.com/poplution-countries-yearwise.json"
                );
                let data = await res.json();
                showOption(data);
                getValue(data);
            };
            getData();
            // console.log(Newdata);
        </script>
    </body>
</html>