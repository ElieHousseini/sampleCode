# sampleCode
This is a sample of the library that includes helper methods for Javascript


var GETReq = function (url, data, successHandler, completeHandler) {
  var xmlhttp
  if (window.XMLHttpRequest) {
    xmlhttp = new XMLHttpRequest()
  } else {
    xmlhttp = new ActiveXObject('Microsoft.XMLHTTP')
  }
  xmlhttp.onreadystatechange = function () {
    if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
      var data = xmlhttp.responseText
      try {
        data = JSON.parse(data)
        // alert(data.count)
      } catch (e) {}
      successHandler && successHandler(data)
    }
  }
  xmlhttp.open('GET', url, true)
  xmlhttp.send()

  xmlhttp.onload = completeHandler
}


function loadScript(url, callback) {
  var head = document.getElementsByTagName('head')[0]
  var script = document.createElement('script')
  script.type = 'text/javascript'
  script.src = url
  script.onload = callback
  head.appendChild(script)
}

function hideElements(elements) {
  for (let i = 0; i < elements.length; i++) elements[i].style.display = 'none'
}

Element.prototype.hide = function () {
  this.style.display = 'none'
}

Element.prototype.show = function () {
  this.style.display = 'block'
}

function removeElements(element) {
  for (let i = 0; i < element.length; i++)
    element[i].parentNode.removeChild(element[i])
}

Element.prototype.remove = function () {
  this.parentNode.removeChild(this)
}

Array.prototype.removeAttributes = function (attribute) {
  Array.forEach(function (el) {
    el.removeAttribute(attribute)
  })
}

Element.prototype.hasClass = function (className) {
  return this.className.indexOf(className) > -1 ? true : false
}

Element.prototype.addClass = function (className) {
  if (!this.hasClass(' ' + className + ' ')) {
    // console.log(className)
    this.className += ' ' + className
  }
  return this
}

Element.prototype.removeClass = function (className) {
  this.className = this.className.replace(
    new RegExp('\\b' + className + '\\b'),
    ''
  )
  return this
}

Element.prototype.addCss = function (property, value) {
  this.style.property = value
  return this
}

Element.prototype.toggleClass = function (className) {
  this.hasClass(className)
    ? this.removeClass(className)
    : this.addClass(className)
  return this
}

Element.prototype.toggle = function () {
  checkVisibility(this) ? this.show() : this.hide()
}

function getIndexOfElement(parent, element) {
  var index = [].indexOf.call(parent, element)
  return index
}

Element.prototype.isVisible = function () {
  //getComputedStyle(element, null);
  if (
    this.style.display == 'none' ||
    (this.offsetWidth == 0 && this.offsetHeight == 0)
  )
    return false
  else return true
}

function live(eventType, elementQuerySelector, cb) {
  document.addEventListener(eventType, function (event) {
    var qs = document.querySelectorAll(elementQuerySelector)

    if (qs) {
      var el = event.target,
        index = -1
      while (el && (index = Array.prototype.indexOf.call(qs, el)) === -1) {
        el = el.parentElement
      }

      if (index > -1) {
        cb.call(el, event)
      }
    }
  })
}

function findChild(parent, children) {
  if (document.getElementById(parent).getElementsByClassName(children).length)
    return document.getElementById(parent).getElementsByClassName(children)
      .length
  else return 0
}

function serializeForm(form) {
  var inputs = form.querySelectorAll('input,select')
  var serializedUrl = []
  for (let i = 0; i < inputs.length; i++) {
    if (
      inputs[i].getAttribute('type') != 'submit' &&
      inputs[i].getAttribute('type') != 'button'
    )
      serializedUrl.push(inputs[i].getAttribute('name') + '=' + inputs[i].value)
  }

  return serializedUrl.join('&')
}

Element.prototype.appendBefore = function (element) {
  element.parentNode.insertBefore(this, element)
},
  false;

/* Adds Element AFTER NeighborElement */
Element.prototype.appendAfter = function (element) {
  if (element != null && element.nextSibling != null)
    element.parentNode.insertBefore(this, element.nextSibling)
},
  false;

Element.prototype.child = function (children) {
  var child = this.querySelectorAll(children)
  return child.length ? child[0] : false
}

Element.prototype.findParent = function (parentClass) {
  element = this
  var elementParent = element.parentElement

  while (elementParent) {
    if (!elementParent.matches(parentClass))
      elementParent = elementParent.parentElement
    else if (elementParent == 'undefined') {
      return false
    } else return elementParent
  }
  return false
}

Element.prototype.sibling = function (sibling) {
  element = this
  var elementParent = element.parentElement
  while (elementParent) {
    var elementParent = element.parentElement
    if (elementParent == null) {
      return false
    }
    siblingElement = elementParent.child(sibling)
    if (elementParent.child(sibling)) {
      return siblingElement
    } else {
      element = elementParent
    }
  }
}
function delegate(el, evt, sel, handler) {
  if (el != null && el != 'undefined') {
    el.addEventListener(evt, function (event) {
      var t = event.target
      while (t && t !== this) {
        if (t.matches(sel)) {
          handler.call(t, event)
        }
        t = t.parentNode
      }
    })
  }
}

Element.prototype.on = function (event, targetElement, callback) {
  this.addEventListener(event, function (e) {
    targetClicked = false

    if (
      e.target.className.indexOf(targetElement) > -1 ||
      e.target.tagName == targetElement
    )
      targetClicked = e.target
    else if (e.target.parentNode.className.indexOf(targetElement) > -1)
      targetClicked = e.target.parentNode

    if (targetClicked) {
      callback(targetClicked)
    }
  })
}

function isNumeric(n) {
  return !isNaN(parseFloat(n)) && isFinite(n)
}

function touchstart(Class, callback) {
  for (let i = 0; i < Class.length; i++) {
    Class[i].addEventListener('touchstart', function () {
      callback(this)
    })
  }
}

function touchend(Class, callback) {
  for (let i = 0; i < Class.length; i++) {
    Class[i].addEventListener('touchend', function () {
      callback(this)
    })
  }
}

function getElementWidth(element) {
  if (element.offsetWidth == 0 && element.offsetHeight == 0) {
    element.style.left = '-1023px'
    element.style.display = 'block'
    var calWidth = element.offsetWidth
    element.removeAttribute('style')
    return calWidth
  } else {
    return element.offsetWidth
  }
}

function toggleSlide(element, time, callback) {
  isVisible(element)
    ? slideLeft(element, time, callback)
    : slideRight(element, time, callback)
}

function slideRight(element, time, callback) {
  console.log('show')
  var elementWidth = getElementWidth(element)
  element.style.overflow = 'hidden'
  element.style.width = '0px'
  element.style.display = 'block'
  var animation = setInterval(function () {
    element.style.width = parseInt(element.style.width) + time + 'px'
    if (parseInt(element.style.width) >= elementWidth) {
      clearInterval(animation)
      element.style.overflow = ''
      element.style.width = ''
      callback()
    }
  }, 1)
}

function slideLeft(element, time, callback) {
  element.style.overflow = 'hidden'
  element.style.width = getElementWidth(element) + 'px'
  console.log(element.style.width)
  var animation = setInterval(function () {
    element.style.width = parseInt(element.style.width) - time + 'px'
    if (parseInt(element.style.width) <= 0) {
      clearInterval(animation)
      element.style.display = 'none'
      element.style.overflow = ''
      element.style.width = ''
      callback()
    }
  }, 1)
}

/** Scroll to the top of the page with duration  **/
function scrollToTop(scrollDuration) {
  var scrollStep = -window.scrollY / (scrollDuration / 15),
    scrollInterval = setInterval(function () {
      if (window.scrollY != 0) {
        window.scrollBy(0, scrollStep)
      } else clearInterval(scrollInterval)
    }, 15)
}

/**
 * Add a cookie using javascript
 *
 * @param cname
 * @param cvalue
 * @param exdays
 */
function setCookie(cname, cvalue, exdays) {
  var d = new Date()
  d.setTime(d.getTime() + exdays * 24 * 60 * 60 * 1000)
  var expires = 'expires=' + d.toUTCString()
  document.cookie = cname + '=' + cvalue + '; ' + expires + '; path=/'
}

/**
 * Retrieve a cookie using javascript
 *
 * @param cname
 * @returns {string}
 */
function getCookie(cname) {
  var name = cname + '='
  var ca = document.cookie.split(';')
  for (var i = 0; i < ca.length; i++) {
    var c = ca[i]
    while (c.charAt(0) == ' ') {
      c = c.substring(1)
    }
    if (c.indexOf(name) == 0) {
      return c.substring(name.length, c.length)
    }
  }
  return ''
}

/**
 * Return true whenever elem is visible in viewport
 *
 * @param elem
 * @returns {boolean}
 */
function inCenter(elem) {
  var scroll_bottom = window.scrollY + window.innerHeight
  var scroll_top = window.scrollY
  return (
    (scroll_bottom > elem.offsetTop &&
      scroll_top < elem.offsetTop + elem.offsetHeight) ||
    false
  )
}

function is(elem, selector) {
  //elem is an element, selector is an element, an array or elements, or a string selector for `document.querySelectorAll`
  if (selector.nodeType) {
    return elem === selector
  }

  var qa =
      typeof selector === 'string'
        ? document.querySelectorAll(selector)
        : selector,
    length = qa.length,
    returnArr = []

  while (length--) {
    if (qa[length] === elem) {
      return true
    }
  }

  return false
}

function isNumeric(n) {
  return !isNaN(parseFloat(n)) && isFinite(n)
}

function removeElements(element) {
  for (let i = 0; i < element.length; i++)
    element[i].parentNode.removeChild(element[i])
}

function indexInParent(node) {
  var children = node.parentNode.childNodes
  var num = 0
  for (var i = 0; i < children.length; i++) {
    if (children[i] == node) return num
    if (children[i].nodeType == 1) num++
  }
  return -1
}

Element.prototype.findParentBySelector = function (parentClass) {
  element = this
  var elementParent = element.parentElement

  while (elementParent) {
    if (!elementParent.matches(parentClass))
      elementParent = elementParent.parentElement
    else if (elementParent == 'undefined') {
      return false
    } else return elementParent
  }
  return false
}
