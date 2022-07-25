// an E-wallet program written in javascript

const users = {}
let initialised = false;
let currentId = 2;

function userValid(id){
    console.log(users)
    if(users[id]){
        if(users[id]?.exist){
            return true
        }
        return false
    }
}

function calculateFee(amount){
    let fee;
    if(amount < 10000){
        fee = 0
    }
    else if(amount >= 10000 && amount <= 100000){
        fee = 200
    }
    else{
        fee = 1000
    }
    return fee
}

function transferMoney(){
    let fromUserId = Number(document.getElementById('from').value);
    let toUserId = Number(document.getElementById('to').value);
    let amount = Number(document.getElementById('amount').value);
    let button = document.getElementById('transferButton');
    let result = document.getElementById('resultTransferMoney');

    button.addEventListener('click', (e)=>{
        e.preventDefault
        const feesToPay = calculateFee(amount);
        if(userValid(fromUserId) && userValid(toUserId)){
            if((users[fromUserId].balance + feesToPay) >= amount && amount > 0){
                users[fromUserId].balance = users[fromUserId].balance - (amount + feesToPay)
                users[toUserId].balance = users[toUserId].balance + amount
                users[1].balance += feesToPay
                result.innerText = 'transfer completed'
            }
            else{
                result.innerText = amount > 0 ? 'user does not have enough balance' : 'can not send amount below 1'
            }
        }
        else{
            result.innerText = users[fromUserId].exist ? 'sender does not exist' : 'the receiver you selected does not exist'
        }
    })

}

function init(){
    users[1] = {
        id: 1, 
        name: 'admin', 
        balance: 1000, 
        exist: true
    }
    initialised = true;
}

function createAccount(){
    let name = document.getElementById('name').value;
    let button = document.getElementById('accountButton');
    let result = document.getElementById('resultAccountCreation');

    button.addEventListener('click', (e)=>{
        e.preventDefault
        let newUser = {id:currentId, name: '', balance: 0, exist: false}
        newUser.name = name;
        newUser.id = currentId;
        newUser.balance = 1000;
        newUser.exist = true;
        users[currentId] = newUser;
        currentId += 1;

    })
    result.innerText = `user added successfully, user ID is ${currentId} `

    console.log(users)
}

function readBalance () {
    let user = document.getElementById('user').value;
    let button = document.getElementById('userButton');
    let result = document.getElementById('resultBalance');

    button.addEventListener('click', (e)=>{
        e.preventDefault
    })
    console.log(users)
    result.innerText = `${users[user]?.name} has ${users[user]?.balance} left`
}

if(initialised === false){
    init()
}

