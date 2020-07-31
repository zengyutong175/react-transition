# react-transition
react transition 实现

## demo
<Transition
    style={{
        padding: 100,
    }}
    transition="all 0.2s cubic-bezier(0.62, 0.38, 0.25, 1) 0s"
    newItemStyle={{ opacity: 0 }}
    list={[{key:1,value:1},{key:2,value:2}]}
    item={(value) => <span className="box">{value.value}</span>}
/>
