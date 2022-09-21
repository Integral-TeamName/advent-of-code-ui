<script>
	let name = 'AOC';
	let answer = '';

	async function handleSubmit(e) {
		const formData = new FormData(e.target);
        let part = '';
        let data = {};
        for (let field of formData) {
            const [key, value] = field;
            data[key] = value;
            if (key === 'a' && value === 'on') {
              part = 'a';
            }
            if (key === 'b' && value === 'on') {
              part = 'b';
            }
        }
        await fetch(
            `http://localhost:8090/solve/${data.year}/${data.day}?part=${part}`,
            {method: 'POST', body: data.input}
        ).then(async res => {
          const resText = await res.text()
          answer = resText.match(/(?<=part [a|b]: )\w+/)
        });
    }

</script>

<h1>Hello {name}!</h1>

<form on:submit|preventDefault={handleSubmit}>
	<label for="year">year</label>
	<input
		id="year"
		name="year"
		type="number"
		min="1970"
		max={new Date().getUTCFullYear()}
		value="2016"
	/>

	<label for="day">day</label>
	<input id="day" name="day" type="number" min="1" max="31" value="1" />

	<label for="input">input</label>
	<textarea
		id="input"
		name="input"
		cols="30"
		rows="15"
		placeholder="enter puzzle input here"
    ></textarea>

	<label for="a">a</label>
	<input id="a" name="a" type="checkbox" checked />

	<label for="b">b</label>
	<input id="b" name="b" type="checkbox" />

	<button type="submit"> solve!</button>
</form>

<p>
	Answer: {answer}
</p>
