<script>
  import { getContext } from 'svelte';
  import '@fullcalendar/core/locales-all';
  import FullCalendar from 'svelte-fullcalendar';
  import daygridPlugin from '@fullcalendar/daygrid';
  import timeGridPlugin from '@fullcalendar/timegrid';
  import listPlugin from '@fullcalendar/list';
  import interactionPlugin from '@fullcalendar/interaction';
  import { onMount } from 'svelte';
  import { langs, codeLang } from './lang';

  export let language;
  export let calendarClick;
  export let calendarDrop;

  export let mappingId;
  export let mappingTitle;
  export let mappingDate;
  export let mappingStart;
  export let mappingEnd;

  export let mappingId2;
  export let mappingTitle2;
  export let mappingDate2;
  export let mappingStart2;
  export let mappingEnd2;

  export let dataProvider;
  export let dataProvider2;

  export let mappingColor;
  export let mappingColor2;

  export let allday;
  export let allday2;

  export let headerOptionsStart;
  export let headerOptionsCenter;
  export let headerOptionsEnd;

  let eventsList = [];
  onMount(() => {
    if (eventsList.length > 0) {
      eventsList = [];
    }
    if (dataProvider.rows) {
      dataProvider.rows.forEach((event) => {
        let eventColor = mappingColor ?? '#313131';
        eventsList.push({
          id: parseInt(event[mappingId]),
          title: event[mappingTitle],
          date: event[mappingDate],
          start: event[mappingStart],
          end: event[mappingEnd],
          color: eventColor,
          event: event,
          allDay: allday,
          editable: true,
        });
      });
    }
    if (dataProvider2.rows) {
      dataProvider2.rows.forEach((event) => {
        let eventColor2 = mappingColor2 ?? '#eb4034';
        eventsList.push({
          title: event[mappingTitle2],
          date: event[mappingDate2],
          start: event[mappingStart2],
          end: event[mappingEnd2],
          color: eventColor2,
          event: event,
          allDay: allday2,
          editable: true,
        });
      });
    }
    eventsList = eventsList;
  });

  let options = {
    headerToolbar: {
      start: headerOptionsStart,
      center: headerOptionsCenter,
      end: headerOptionsEnd,
    },
    plugins: [daygridPlugin, listPlugin, timeGridPlugin, interactionPlugin],
    initialDate: Date.now(),
    locale: language,
    dayMaxEvents: true,
    eventClick: (event) => {
      calendarClick({
        value: event.event,
      });
      console.log(JSON.parse(text));
      console.log(event.event.title);
    },
    eventDrop: (event) => {
      calendarDrop({
        value: event.event,
      });
      console.log(JSON.parse(text));
      console.log(event.event.title);
    },
    events: eventsList,
    eventColor: '#378006',
    theme: true,
    ...langs[codeLang(language)],
  };
  const { styleable } = getContext('sdk');
  const component = getContext('component');
</script>

<div use:styleable={$component.styles}>
  <FullCalendar {options} />
</div>
