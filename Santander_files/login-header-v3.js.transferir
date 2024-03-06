//VERSION: 3.0

var loginForm = document.querySelector("form"),
    loginButtonPf = document.querySelectorAll("icon-circle-arrow")[1],
    loginButtonPj = document.querySelector("icon-circle-arrow-pj");
let loginType = "login-pf";

function fMasc(e, t) {
    obj = e, masc = t, setTimeout("fMascEx()", 1)
}

function fMascEx() {
    obj.value = masc(obj.value)
}

function mCPF(e) {
    return 14 == (e = (e = (e = (e = e.replace(/\D/g, "")).replace(/(\d{3})(\d)/, "$1.$2")).replace(/(\d{3})(\d)/, "$1.$2")).replace(/(\d{3})(\d{1,2})$/, "$1-$2")).length && validCPF(e), e
}

function validCPF(e) {
    var t, n, o = e.replace(/[.-]/gm, "");
    if (t = 0, "" == o) return !1;
    if ("00000000000" == o || "11111111111" == o || "22222222222" == o || "33333333333" == o || "44444444444" == o || "55555555555" == o || "66666666666" == o || "77777777777" == o || "88888888888" == o || "99999999999" == o) return alert("CPF inválido!"), !1;
    for (i = 1; i <= 9; i++) t += parseInt(o.substring(i - 1, i)) * (11 - i);
    if (10 != (n = 10 * t % 11) && 11 != n || (n = 0), n != parseInt(o.substring(9, 10))) return alert("CPF inválido!"), !1;
    for (t = 0, i = 1; i <= 10; i++) t += parseInt(o.substring(i - 1, i)) * (12 - i);
    return 10 != (n = 10 * t % 11) && 11 != n || (n = 0), n == parseInt(o.substring(10, 11)) || (alert("CPF inválido!"), !1)
}

function onlynumber(e) {
    var t = e.charCode ? e.charCode : e.keyCode;
    if (8 != t && 9 != t && (t < 48 || t > 57)) return !1
}

function exchangeIconLock(e) {
    "txtCPF" === e ? (document.getElementsByClassName("login-icon icon-lock")[1].style.display = "none", document.getElementsByTagName("icon-circle-arrow")[1].style.display = "block") : "txtConta" === e && (document.getElementsByTagName("icon-lock-company")[0].style.display = "none", document.getElementsByTagName("icon-circle-arrow-pj")[0].style.display = "block")
}

function exchangeIconArrow(e) {
    "txtCPF" === e ? "" === document.getElementsByName("txtCPF")[0].value && (document.getElementsByClassName("login-icon icon-lock")[1].style.display = "block", document.getElementsByTagName("icon-circle-arrow")[1].style.display = "none") : "txtConta" === e && "" === document.getElementsByName("txtConta")[0].value && (document.getElementsByTagName("icon-lock-company")[0].style.display = "block", document.getElementsByTagName("icon-circle-arrow-pj")[0].style.display = "none")
}

function insertValueAccount() {
    if(4 == document.getElementsByName("txtAgencia")[0].value.length){
        document.getElementsByName("txtConta")[0].focus()
    }
}

function getCryptographicContext() {
    return new Promise(function (e, t) {
        window.crypto = {}, window.crypto.publicKey = window.crypto.publicKey ? window.crypto.publicKey : DLECC.init();
        var n = window.crypto.publicKey;
        fetch("https://esbapi.santander.com.br/cryptographic-security/v1/key/public/js?gw-app-key=17379590e58f01341baa005056906329", {
            method: "POST",
            body: JSON.stringify({
                clientPublicKey: n,
                system: "WPC"
            })
        }).then(function (t) {
            e(t.json())
        }).catch(function (e) {
            t(e)
        })
    })
}

function createHash(e) {
    return new Promise(function (t, n) {
        fetch("https://esbapi.santander.com.br/cryptographic-security/v1/create-hash?gw-app-key=17379590e58f01341baa005056906329", {
            method: "POST",
            body: JSON.stringify({
                criptoSystemAcronym: "CRM",
                registerList: {
                    register: [{
                        key: "USER_ID",
                        value: DLECC.encryptToApp(getFormValues(), e.serverPublicKey)
                    }]
                },
                systemAcronym: "WPC",
                ticket: e.ticket
            })
        }).then(function (e) {
            t(e.json())
        }).catch(function (e) {
            n(e)
        })
    })
}

function getUrlFromCredential(e) {
    return new Promise(function (t, n) {
        fetch("https://esbapi.santander.com.br/environment-route-management/v1/getURL?gw-app-key=17379590e58f01341baa005056906329", {
            method: "POST",
            body: JSON.stringify({
                channel: getDdsChannel(),
                credential: DLECC.encryptToApp(getFormValues(), e.serverPublicKey),
                system: "WPC",
                ticket: e.ticket
            })
        }).then(function (e) {
            t(e.json())
        }).catch(function (e) {
            n(e)
        })
    })
}

function getDefaultUrl() {
    return "login-pf" === loginType ? "https://pf.santandernet.com.br/LOGBBR_NS_ENS/ChannelDriver.ssobto?dse_contextRoot=true" : "https://pj.santandernetibe.com.br/ibeweb/"
}

function getIBUrl() {
    return new Promise(function (e, t) {
        var n = getDefaultUrl();
        return e(n);
        //Removido o uso da parte inferior, pois não foi comprovado a utilidade da API getURL
        //Data: 06/02/2022
        e(getCryptographicContext().then(function (e) {
            return createHash(e)
        }).then(function () {
            return getCryptographicContext()
        }).then(function (e) {
            return getUrlFromCredential(e)
        }).then(function (e) {
            return e.URL ? n + "&q=" + e.URL : n
        }))
    })
}

function getDdsChannel() {
    return "login-pf" === loginType ? "IBPF" : "IBE"
}

function getFormValues() {
    return "login-pf" === loginType ? loginForm.elements.txtCPF.value.replace(/[.-]/gm, "") : loginForm.elements.txtAgencia.value + "|" + loginForm.elements.txtConta.value
}

function redirectToIB(e) {
    13 !== e.keyCode && "click" !== e.type || (e.preventDefault(), loginForm.elements.txtCPF.value.replace(/[.-]/gm, "") && getIBUrl().then(function (e) {
        submitIBForm(e)
    }))
}

function redirectToIBPJ(e) {
    13 !== e.keyCode && "click" !== e.type || (e.preventDefault(), loginForm.elements.txtAgencia.value && loginForm.elements.txtConta.value && getIBUrl().then(function (e) {
        submitIBPJForm(e)
    }))
}

function submitIBForm(e) {
    loginForm.action = e;
    var t = document.createElement("input");
    t.name = "dse_applicationName", t.value = "ALP_LOGBBR_Presentacion", t.style.display = "none";
    var n = document.createElement("input");
    n.name = "dse_operationName", n.value = "loginNMW", n.style.display = "none", loginForm.appendChild(t), loginForm.appendChild(n);
    var o = loginForm.querySelector("input");
    o.value = o.value.replace(/[.-]/gm, ""), loginForm.submit()
}

function submitIBPJForm(e) {
    loginForm.action = e, loginForm.submit()
}

function showSegment() {
    var e = document.querySelectorAll(".novaHomePJ"),
        t = document.querySelectorAll(".container-profile")[0],
        n = document.querySelectorAll(".container-profile")[1];
    null != e[0] ? (loginType = "login-pj", n.style.display = "block", t.style.display = "none") : (t.style.display = "block", n.style.display = "none")
}
loginButtonPf.addEventListener("click", redirectToIB), loginButtonPj.addEventListener("click", redirectToIBPJ), loginForm.elements.txtCPF.addEventListener("keyup", redirectToIB), loginForm.elements.txtConta.addEventListener("keyup", redirectToIBPJ), showSegment();