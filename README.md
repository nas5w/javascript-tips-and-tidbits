[Imgur](https://i.imgur.com/mrN2Qmx.png)
<div align="center">
  <img alt="JavaScript" src="https://i.imgur.com/mrN2Qmx.png" width="100px" height="100px" />&nbsp;&nbsp;&nbsp;
  <img alt="Tips & Tidbits" src="https://i.imgur.com/EHbPbzj.png" height="100px" /> 
</div>

---

<p align="center">
  A continuously-evolving compendium of javascript tips based on common areas of confusion or misunderstanding.
</p>

---

# Tips & Tidbits

## Sections

- [Comparison](#comparison)
- Section TBD
- Section TBD

## Comparison

### Comparing Objects

A mistake I see javascript newcomers make is directly comparing objects. Variables are pointing to references to the objects in memory, not the objects themselves! One method to actually compare them is converting the objects to JSON strings. This has a drawback though: object property order is not guaranteed! A safer way to compare objects is to pull in a library that specializes in deep object comparison (e.g., [lodash's isEqual](https://lodash.com/docs#isEqual)).

```javascript
const joe1 = { name: 'Joe' };
const joe2 = { name: 'Joe' };

console.log(joe1 === joe2);
// False
```

## Section TBD

Content

## Section TBD

Content

---

# Contributing

Contributing note
