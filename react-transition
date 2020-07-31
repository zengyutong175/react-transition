import React, { useState, useRef, useLayoutEffect } from 'react';
/**
 *
 * @param {Array} list
 * @param {String} listTag list渲染的标签(如li、div)
 * @param {Function} item(value) 渲染函数，listTag的children
 * @param {String} transition 自定义css的transition属性
 * @param {Object} newItemStyle 新增元素的初始样式
 * @param {String} key list唯一的字段
 *
 *
 */

export default function ({
    list,
    listTag = 'div',
    item,
    transition = 'all .2s',
    newItemStyle = {},
    key = 'key',
    ...other
}) {
    const [oldState, setOldState] = useState({});
    const [invert, setInvert] = useState({});
    const [play, setPlay] = useState(false);
    const [first, setFirst] = useState(true);
    const container = useRef(null);
    function cache() {
        let node = container.current.children;
        let state = {};
        for (let v of node) {
            let info = v.getBoundingClientRect();
            state[v.getAttribute('data-key')] = {
                top: info.top,
                left: info.left,
            };
        }
        setOldState(state);
    }
    useLayoutEffect(() => {
        cache();
        if (first) {
            setFirst(false);
        } else {
            let node = container.current.children;
            let state = {};
            for (let v of node) {
                let info = v.getBoundingClientRect();
                state[v.getAttribute('data-key')] = {
                    top: info.top,
                    left: info.left,
                };
            }
            let map = oldState;
            let invert = {};
            list.forEach((v) => {
                let k = v[key];
                invert[k] = {};
                if (map[k]) {
                    invert[k].top = map[k].top - state[k].top;
                    invert[k].left = map[k].left - state[k].left;
                } else {
                    invert[k] = newItemStyle;
                }
            });
            setInvert(invert);
            setPlay(false);
            requestAnimationFrame(() => {
                setInvert({});
                setPlay(true);
            });
        }
    }, [list]);
    return (
        <div ref={container} id="container" {...other}>
            {list.map((v) => {
                const index = v[key];
                let Node = React.createElement(
                    listTag,
                    {
                        key: index,
                        'data-key': index,
                        style: {
                            display: 'inline-block',
                            transition: play ? transition : 'none',
                            opacity: invert[index]
                                ? typeof invert[index].opacity === 'number'
                                    ? invert[index].opacity
                                    : 1
                                : 1,
                            transform: `translate(${invert[index] ? invert[index].left + 'px' : 0},${
                                invert[index] ? invert[index].top + 'px' : 0
                            })`,
                        },
                    },
                    item(v)
                );
                return Node;
            })}
        </div>
    );
}
